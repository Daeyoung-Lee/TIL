## gitignore

git 저장소로 설정된 폴더의 모든 파일들은 변경사항이 추적된다.

만일, 내가 git으로 관리하고 싶지 않은 파일이 있다면 `.gitignore`파일을 생성하고, 다음과 같이 설정한다.

```bash
*.xlsx # 특정 확장자인 파일
a.txt # 특정 파일
images # 특정 폴더
!upload.xlsx # 특정 파일 제외
```

직접 추가하는 내용 뿐만 아니라 일반적인 개발 환경에서 반드시 설정하는 파일/폴더들이 있다.

​	예) IDE 설정 파일, cache 파일 등..

처음에는 직접 설정하기 힘드니 [gitignore](https://www.gitignore.io/)에서 만들어진 내용을 가져온다.

* `windows` , `node` , `react` , `visualstudiocode`

  