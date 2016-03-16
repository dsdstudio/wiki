## Daylight Saving time 


## Issue 정리 

### 시간대가 바뀌는 지역 system time 일치 문제 

2012년의경우 4월 1일 오전 2시가 될때 PST -> PDT로 바뀌면서 1시간이 더 늦춰진다 
시스템 clock 을 예를 들면  (PST/PDT) 

* `2012-04-01 01:59:59 (PST GMT-8) -> 2012-04-01 03:00:00 (PDT GMT-7)`   
* `2012-10-28 01:59:59 (PST GMT-7) -> 2012-10-28 01:00:00 (PDT GMT-8)`   
와 같은형태로 바뀌어진다. 

이렇게 바뀌는 Timezone을 가진 지역에서는 별도의 `SimpleTimeZone` class를 이용하거나 이 문제가 이미 해결된 `jodatime` library를 이용하자.


 

## joda time에서 timezonerule 사용할때 필요한 옵션 

	-Dorg.joda.time.DateTimeZone.Provider=org.joda.time.tz.UTCProvider

다시 검토해보니 없어도되는거같기도 하고..

## summer time을 사용하는 timezone이고 시간을 보정하는경우 

### jodatime 

PST -> PDT 

	DateTimeZone zone = new DateTimeZoneBuilder()
		.addCutover(-292275054, 'w', 1, 1, 0, false, 0)
		.setStandardOffset(-28800 * 1000).setFixedSavings("WLT", 0)
		.addRecurringSavings("PDT", 3600 * 1000, 2000, 3000, 'w', 4, 1, 7, true, 7200 * 1000)
		.addRecurringSavings("PST", 0, 2000, 3000, 'w', 10, -1, 7, false, 7200 * 1000).toDateTimeZone("WLTZ", false);
	DateTime time = new DateTime(2012, 10, 28, 1, 0, 0, 0, zone);
	System.out.println(time.toString("yyyy-MM-dd HH:mm:ss z"));
	System.out.println(time.plusHours(1).toString("yyyy-MM-dd HH:mm:ss z"));
	
	time = new DateTime(2012, 10, 27, 1, 0, 0, 0, zone);
	System.out.println(time.plusDays(1).toString("yyyy-MM-dd HH:mm:ss z"));
	
	time = new DateTime(2012,3,31,2,0,0,zone);
	System.out.println(time.plusDays(1).toString("yyyy-MM-dd HH:mm:ss z"));
	
	// 결과 
	2012-10-28 01:00:00 -07:00
	2012-10-28 01:00:00 -08:00
	2012-10-28 01:00:00 -07:00
	2012-04-01 03:00:00 -07:00

기준시각 : 10.28 01:00 -07:00   
+1시간 : 10.28 01:00 -08:00 `timezone이 바뀌고 시각은 1시` 

기준시각 : 10.27 01:00 -07:00  
+1일 : 10.28 01:00 -07:00 `timezone은 그대로, 시각은 1시`

기준시각 : 2012-03-31 02:00 -08:00  
+1일 : 2012-04-01 03:00 -07:00 `TZ 변경, 시간 변경`  

### Basic Calendar 

Basic Calendar의 Week 는 jodatime에서 사용하는 Week의 그것과 다르기때문에 아래와 같은 함수를 만들어서 보정값을 얻어내야 한다. 

	public int getBasicCalendarDayOfWeek() {
		switch (this.dayOfWeek) {
		case DateTimeConstants.SUNDAY:
			return Calendar.SUNDAY;
		case DateTimeConstants.SATURDAY:
			return Calendar.SATURDAY;
		case DateTimeConstants.FRIDAY:
			return Calendar.FRIDAY;
		case DateTimeConstants.THURSDAY:
			return Calendar.THURSDAY;
		case DateTimeConstants.WEDNESDAY:
			return Calendar.WEDNESDAY;
		case DateTimeConstants.TUESDAY:
			return Calendar.TUESDAY;
		case DateTimeConstants.MONDAY:
			return Calendar.MONDAY;
		default:
			return Calendar.SUNDAY;
		}
	}

또한 `TimeZone` 세팅은 아래와 같이 하였다. 수도코드이니 참고만 하기바란다.

	SimpleTimeZone zone = new SimpleTimeZone(standardoffset * 1000, "WLT");
	zone.setStartRule(startset.getMonthOfYear() - 1, startset.getDayOfMonth(), startset.getBasicCalendarDayOfWeek(), startset.getMillsOfDay() * 1000);
	zone.setEndRule(endset.getMonthOfYear() - 1, endset.getDayOfMonth(), endset.getBasicCalendarDayOfWeek(), endset.getMillsOfDay() * 1000);
	TimeZone.setDefault(zone);

## Schedule time 보정 

다른 Timezone의 경우도 비슷하겠지만 아래 보정값은 (PST/PDT) 기준이다. 

### DateTimeZone 에서 꼭 숙지해야할 Reference 

* [DateTimeZone.isFixed()](http://joda-time.sourceforge.net/api-release/src-html/org/joda/time/DateTimeZone.html#line.1202) : 시간에 따라 변하는 Timezone인지 검사 
* [DateTimeZone.nextTransition(long instant)](http://joda-time.sourceforge.net/api-release/src-html/org/joda/time/DateTimeZone.html#line.1212) : 현재시간 기준으로 TZ가 바뀌는 다음시간 timestamp
* [DateTimeZone.previousTransition(long instant)](http://joda-time.sourceforge.net/api-release/src-html/org/joda/time/DateTimeZone.html#line.1222) : 현재시간 기준으로 TZ가 바뀌는 바로 전시간 timestamp

### 들어오는날 처리 방안 
들어오는 날인경우 `2012-04-01 02:xx ~ 2012-04-01 02:59` 실제 2시가 없다 1:59분 뒤 바로 3시로 간다. 

이런경우 2시대 스케줄이 걸린 작업들은 + 1시간 보정해서 3시에 실행될수있도록 한다.   
예 ) 스케줄걸린시간 -> `02:30`  실제 실행시간 -> `03:30`
 
	DateTime now = DateTime.now(zone);
	DateTime startday = zone.previousTransition(now.getMills());
	int schedulehour = ctx.getScheduleHour();
	// 들어오는 시간대인지 확인하는 boolean 함수가 필요 
	
	// timezone rule에서 offset을 가져올수있어야함
	// 이전 시간대와 날짜가 같고 시간 +1 한것과 동일하면 ? 
	if ( !now.isFixed() && now.toString("yyyy-MM-dd")) {
		// 스케줄 날짜 확인
	}
	
### 나가는날 처리방안 

나가는 날인 `2012-10-28 01:59 ~ `에는 1시가 2번 오게된다.   
이런경우 1시에 1시에 걸린 작업들이 이미 실행되었으면 실행되지않도록 처리한다. 

예) 스케줄 걸린시간 -> `01:30` 실제 실행시간 -> `01:30` 나가는 TZ에서는 실행하지않도록 합니다. 


## References

[Summertime 참고 사이트](http://www.timeanddate.com/time/dst/events.html)

