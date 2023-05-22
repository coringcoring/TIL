# week 2

* 라우트 함수: annotation으로 매핑되는 함수
* 블루프린트: 라우트 함수를 구조적으로 관리 가능 
* URL 프리픽스는 접두어 URL을 정할 때 사용함 

* ORM(Object Relational Mapping)을 사용하여 파이썬 문법만으로도 데이터베이스 다룰 수 있음 
    * 데이터베이스 종류와 상관없이 일관된 코드를 유지할 수 있어서 프로그램 유지/보수하기가 편리함 
* flask db init 명령은 최초 한번만 수행하면 됨 
* `flask db migrate`: 모델을 새로 생성하거나 변경할 때 사용
* `flask db upgrade`: 모델의 변경 내용을 실제 데이터베이스에 적용할 때 사용 

* 템플릿 태그 : 파이썬 코드와 연결됨 
    * `{% if question_list %}` : render_template 함수에서 질문 목록 데이터 question_list가 있는지 검사 
    * `{% for question in question_list %}`: question_list에 저장된 데이터를 하나씩 꺼내 question 객체에 대입함 
