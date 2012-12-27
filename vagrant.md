## vagrant & chef 를 이용한 가상환경 

## 왜 쓰는가 ? 

개발자 환경 == 운영환경 == CI 서버 환경, 환경설정에 따른 시간낭비 방지를 위해서이다. 또한 환경때문에 하는 삽질을 줄이기 위함이다 


### 작업환경 만들기 

	$ mkdir vm 
	$ vagrant init 
	$ vagrant box add base https://dl.dropbox.com/u/1543052/Boxes/UbuntuServer12.04amd64.box 
	[vagrant] Downloading with Vagrant::Downloaders::HTTP...
	[vagrant] Downloading box: https://dl.dropbox.com/u/1543052/Boxes/UbuntuServer12.04amd64.box
	[vagrant] Progress: 0% (2457282 / 279808512)

기본적으로 사용되는 vm os를 `ubuntu 12.04 amd64`로 설정하였다. 이밖에 다른 여러 이미지들도 [http://www.vagrantbox.es/](http://www.vagrantbox.es/) 에서 구할수있다. 

### 환경에 기본적으로 설치되어야하는 프로그램 설정 

chef cookbook 들로 해결할수있다. repository에서 자신이 세팅해야할 Application 에 해당하는 cookbook 을 clone 하자. 

여기에서는 mysql을 예로 들었다. 

	$ mkdir cookbooks 
	$ git clone https://github.com/opscode-cookbooks/mysql



## References 

- [vagrantboxes](http://www.vagrantbox.es/)
- [vagrant document](http://vagrantup.com/v1/docs/index.html)
- [vagrant cookbooks](https://github.com/opscode-cookbooks)
