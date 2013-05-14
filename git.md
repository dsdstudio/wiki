## 커밋 합치기 

	$ git rebase -i HEAD~4

### reference 
- [http://ko.gitready.com/advanced/2009/02/10/squashing-commits-with-rebase.html](http://ko.gitready.com/advanced/2009/02/10/squashing-commits-with-rebase.html)

## 커밋 메시지 고치기 

	$ git commit --amend

## Repository line count 

	$ git ls-files | xargs wc -l


	# 전체 Code line 수 
	$ git ls-files | xargs wc -l

## reset single file 

repository로 부터 최신버전으로 fetch & merge 시 충돌이나는경우 사용한다. 

하나의 파일을 예전으로 돌리려는경우 

	$ git checkout -- filename


현재 커밋되었고, remote로 push 하지 않은부분까지 모두 Unstaging 상태로 만들려면  

	$ git reset --HARD 


## Tag, branch push 하기 

	git push 원격저장소명 로컬브랜치명
	git push 원격저장소명 로컬브랜치명:원격브랜치명
	
	$ git push origin branchname
	
