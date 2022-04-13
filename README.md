# 에러모음집(내가 코딩하는 과정에서 생긴 오류 및 난제들을 모아두는 저장소)

## 1. e/storageexception: { "error": { "code": 404, "message": "not found." }}
 - 환경 : Android, firebase, kotlin
 - 원인 및 문제점 : 사용자가 작성한 게시글을 수정하면서 기존에 스토리지에 저장된 업로드한 이미지를 삭제하는 과정에서 발생한 오류. 결론적으로는 삭제할 사진이 스토리지에 존재하지 않았기 때문에 발생한 오류. 수정하기버튼을 터치시 startActivity를 활용한 화면전환이 아닌 super.onBackPress()를 사용했기 때문에 서버에서 수정된 데이터를 불러오는게 아니라 기존의 데이터를 그대로 화면에 띄워주고있었던것.(startActivity를 활용했을때, recyclerView의 item을 터치해서 들어온 게시글의 detail페이지라 starActivity로 이동하면 지정했던 게시글의 item을 찾을수 없었다.) super.onBackPress()를 통해 해당 액티비티로 돌아갔을때 새로고침을 하는 방안을 고려했지만 무한새로고침의 문제점이 발생.<br>
 - 해결방법 : singleton을 사용한다. recyclerView의 item을 터치시 이벤트에 singleton에 해당 게시글의 seq번호를 저장하고 activity의 데이터 설정을 singleton에서 꺼내와서 세팅을 해준다. 그리고나서 super.onBackPress()를 사용하지 않고 startActivity를 통해 다시 게시글정보로 이동하면 지정했던 게시글도 불러올 수 있고 스토리지에 사진이 삭제되지 않는 문제도 해결할 수 있다.


## 2. java.lang.NullPointerException: Cannot invoke "health.back.a.service.WorkBbsService.getBbsList()" because "this.sv" is null
 - 환경 : Spring Boot
 - 원인 및 문제점 : Spring Boot로 서버를 켜고 웹페이지에서 게시판을 불러오는 도중에 500에러가 발생했다. 오류내용에서도 보이듯 this.sv(WorkBbsService sv)가 null이라는 내용이다. 
Controller에서 Service를 찾지 못하는 상황.
 - 해결방법 : 처음에는 매개변수나 Dao에서 값을 잘못받아오는줄 알고 코드를 한참 보았다. 알고보니 간단한 실수였다. @AutoWired가 없었다. @Autowired는 스프링에서 사용되는 어노테이션인데 설정한 메서드가 자동으로 호출되고, 인스턴스가 자동으로 주입이 된다. 즉 @AutoWired가 없어서 Service가 호출되지않아 null값이 나왔던 것이었다.


## 3. java.lang.NullPointerException: Attempt to invoke virtual method 'void android.support.v7.widget.RecyclerView.setAdapter(android.support.v7.widget.RecyclerView$Adapter)' on a null object reference
 - 환경 : Android, kotlin
 - 원인 및 문제점 : recyclerView에 어댑터를 설정하는 과정에서 생긴 오류. 기간검색 후 불러온 정보를 recyclerView로 띄우려고 하는데 서버에서는 문제없이 데이터가 넘어오는데 adapter가
 계속 null이 나오면서 오류가 발생했다.
 - 해결방법 : 아무래도 팀프로젝트 협업이다보니 View의 id가 겹치거나 비슷했던게 원인이었다. recyclerview라는 id를 가진 View에 adapter를 적용해야하는데 recyclerView라는 id를 적었었다.


## 4. java.lang.IllegalArgumentException: Non-body HTTP method cannot contain @Body.
 - 환경 : Android, kotlin, retrofit2
 - 원인 및 문제점 : retrofit2를 이용하여 서버와 통신하던 중에 발생한 에러. 원인은 @GET방식을 사용하고 POST방식으로 서버로 데이터를 전달했던것이 문제였다.
 - 해결방법 : 데이터 통신 방식을 @GET 과 @POST 둘중 하나만 사용한다.
