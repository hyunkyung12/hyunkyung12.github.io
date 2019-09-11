---
layout: post
title: '[딥러닝 모델을 사용한 서버 배포하기 #3] Flask로 서버 개발하기'
author: hyunkyung
date: 2019-09-11 15:38
tags: [deep_learning, flask]
image: 
---

# Keras모델을 사용하는 Flask서버 개발하기

앞의 포스팅에서 Tensorflow 모델을 Keras 모델로 바꾸어 사용하는 방법을 알아보았습니다.

이제는 이 Keras 모델을 사용해 어떻게 서버를 만드는지 알아보도록 하겠습니다. 

> Flask의 기본 개념에 대한 내용은 생략합니다. 기본 개념은 [공식문서](http://flask.palletsprojects.com/en/1.1.x/)를 참고하세요!



미리보기)

![](/files/flask-server.png)

만들 서버의 모습입니다! 간단한 UI와 ~~멋있는 사진~~ 결과를 볼 수 있습니다! <br>입력으로 이미지의 URL을 받아 Keras 모델을 사용해 결과물을 얻고, 그 결과를 웹 화면에 보여주는 방식입니다.

순서는 

1. flask 코드 작성
2. json 값을 이용해 html 화면에 결과 띄우기

로 진행하겠습니다.



### flask 코드 작성

우선은 `app.py ` 파일을 작성합니다. (flask를 실행할 파일)

```python
app = flask.Flask(__name__)

@app.route("/", methods=["GET"])
def main():
    return render_template('index.html')
```

main 함수에서는 index 페이지만 보여주도록 합니다.



```python
@app.route("/model", methods=["GET"])
def model():
	if request.method == "GET":
        # 1. url을 입력으로 받아서
		url = request.args.get('url')
		# 2. url을 이미지로 변환한 후
        image = url_to_image(url)
        # 3. Keras 모델의 input으로 사용
		result_file = test_model(image)
        # 4. json 형태로 리턴
		result_dict = {
						'url' : url,
						'result' : encode_image(result_file)
					  }

		return json.dumps(result_dict)
```

이번에는 실제로 모델을 사용하는 함수를 작성하겠습니다.



**1. 이미지의 url을 입력으로 받음**

`request.args.get('url')` 이라고 하면, `/model?url=` 뒤에 들어가는 url값을 가져옵니다.<br>예를들어 /model?url=http://zum.com/ 이라는 입력이 들어오면 url 변수에는 'http://zum.com' 이 들어갑니다.



**2. 이미지의 url을 이미지 배열로 변환**

```python
def url_to_image(url):

	resp = urllib.request.urlopen(url)
	image = np.asarray(bytearray(resp.read()), dtype="uint8")
	image = cv2.imdecode(image, cv2.IMREAD_COLOR)
	
	return image
```

url을 읽어 array로 변환하고, 3차원 형태의 이미지 배열로 다시한번 변환하는 코드 입니다.



**3. 이미지 배열을 모델의 input으로 사용해 모델 테스트**

`model.py`

```python
model = load_model('./model_complete.h5', custom_objects={'tf': tf})
model.load_weights('./model_weights.h5')
model._make_predict_function() # predict 할 때 매번 쓰기 위해 사용하는 함수

def test_model(image):
	output = model.predict(image)
	
	filename = 'temp.png'
	cv2.imwrite(filename, image)
	
	return filename
```

위 코드는 `model.py`에 작성한 내용입니다. Flask에서 일반적으로 route를 가진 함수가 아니면 다른 파일로 분리해 작성하는 방식을 사용하기 때문에, 따로 분리해서 작성했습니다.

여기서 중요한 두 가지가 있는데, 첫번째로 중요한 것은 `model.py` 안에서 모델을 로드한다는 점입니다!<br>`model.py` 내에서 모델을 로드하면, `app.py` 실행 시 맨 위에서 `model.py`를 한번만 부르기 때문에 앱 실행동안 **한번만** 모델을 로드할 수 있습니다.

만약 `app.py`내의 라우트를 가진 함수들 내에서 모델을 로드하면, 함수를 매번 호출할 때 마다 모델 파일을 읽기 때문에 매우 비효율적인 앱이 될 것입니다.

두 번째로는 `model._make_predict_function()` 부분입니다. <br>`model.predict()`를 하기 전에 호출을 해야 predict를 정상적으로 할 수 있습니다.<br>

> 정확한 사용 이유는 공식 문서에도 잘 나와있지 않습니다. Keras github Issue들에도 여러 질문이 있지만, 아직 명확한 답변은 나와있지 않습니다. 답변이 추가되면 수정하겠습니다!



**4. 결과를 json 형태로 리턴**

```python
result_dict = {
                'url' : url,
                'result' : encode_image(result_file)
			  }

return json.dumps(result_dict)
```
매번 이미지 결과를 얻어서 이미지로 저장을 하면 저장공간 상의 문제가 될 수도 있어, `encode_image`라는 함수 내에서 이미지를 인코딩 한 후 인코딩 값만 리턴하고 파일은 삭제하도록 구현을 했습니다.

`json`형식으로 리턴을 하면 서버에서 UI 없이 라우트로만 요청을 보내도 결과를 볼 수 있고, html에서 json형식의 결과를 사용하기 쉽다는 장점이 있습니다.



이제 서버개발은 모두 끝났습니다!<br>`python app.py` 를 실행한 후 `/model?url=[url주소]` 를 입력하게 되면 json 형식의 결과를 볼 수 있습니다!~~~!~!

 

하지만 좀 더 편리하게 사용하기 위해 간단한 UI까지 개발해보도록 하겠습니다!

### 2. json 값을 이용해 html 화면에 결과 띄우기

우선 flask에서 html 파일을 사용하기 위해서는 정해진 디렉토리 규칙이 있습니다.

```
app.py
model.py
templates
	- index.html
```

이런식으로 templates 폴더 내에 html 파일을 넣어주면 별다른 경로 지정 없이 flask가 html파일을 읽을 수 있습니다.



`index.html`

```html
<!DOCTYPE html>
<html>
<head>
	<meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    <title>VIP</title>
</head>
<body>

<h2>saliency map을 구할 이미지의 URL을 입력하세요</h2>
<form class='url'>
	<input type="text" name="url"/>
	<button type="button" id="submit">submit</button>
</form>

<table id="result">
</table>
<script
  src="https://code.jquery.com/jquery-3.4.1.js"
  integrity="sha256-WpOohJOqMqqyKL9FccASB9O0KwACQJpFTUBLTYOVvVU="
  crossorigin="anonymous"></script>
 

<script>

	const Result = ({ url, result }) => `
	<tr>
		<td><img class="crop-result" src="${url}"></td>
		<td><img class="crop-result" src="data:image/png;base64, ${result}"></td>
	</tr>
	`;

	$(function() {
		$('#submit').click(function() {

			var input = $('form.url').serialize();
			console.log(input);

			$.ajax({
				url: '/model',
				data: input,
				type: 'GET',
				success: function(response) {
					var result = JSON.parse(response);
					console.log(result);
					$('#result').html([result].map(Result).join(''));
				},
				error: function(error){
					console.log(error);
				}

			});
		});
	});
</script>
</body>
```

이게 전부입니다..! easy..!



**1. input tag로 입력값 받기**

```html
<form class='url'>
	<input type="text" name="url"/>
	<button type="button" id="submit">submit</button>
</form>

...
<script>
	$(function() {
		$('#submit').click(function() {

			var input = $('form.url').serialize();
...
</script>
```

input tag에 `name='url'`속성을 주었고, form tag의 기본 method는 GET방식이므로, input에 값을 넘겨주면 `?url=값` 으로 넘어가게 됩니다. <br>이 때 값이 넘어가는 시점은 submit 버튼을 누를 때 입니다. 따라서 submit 버튼을 클릭할 때 처리 함수가 작동하도록 하면 됩니다.<br>script 태그 내의 input 변수에 `?url=값` 까지 넘어간 상태 입니다.



**2. 입력받은 값으로 요청 보내기**

```html
<script>
...
$.ajax({
    url: '/model',
    data: input,
    type: 'GET',
...
</script>
```

이제 앞에서 flask로 작성한 함수에 요청을 보낼 차례 입니다.

ajax 형식에 맞게 작성만 하면 됩니다. 어떤 url로 보낼 것인지, 어떤 데이터를 보낼 것인지, 어떤 방식으로 보낼것인지 정의합니다.<br>위와 같이 작성하게 되면 `/model?url=input 값` 형태로 요청이 넘어갑니다.



**3. 요청을 보낸 결과를 받아 파싱해서 뿌리기**

```html
<script>
...
success: function(response) {
    var result = JSON.parse(response);
    $('#result').html([result].map(Result).join(''));
},
error: function(error){
	console.log(error);	
}
...
</script>
```



요청을 보낸 결과를 json 형식으로 정의해 놓았기 때문에 `JSON.parse` 로 쉽게 값을 사용할 수 있습니다.

여기서 한 가지 봐야할 것은 json 형태의 array를 mapping 해서 사용하는 부분입니다.<br>

```html
<table id="result"></table>

<script>
const Result = ({ url, result }) => `
    <tr>
        <td><img class="crop-result" src="${url}"></td>
        <td><img class="crop-result" src="data:image/png;base64, ${result}"></td>
    </tr>
`;
</script>
```

mapping 시 사용한 Result라는 상수는 script의 윗 부분에서 정의했습니다.<br>이 상수 내에서는 json에서 사용하는 key값을 간단히 사용할 수 있습니다.<br>{{  ...  }} 안에 사용할 key 값들을 나열해주고, ${ ... }로 값을 사용해주면 됩니다!



그럼 이제 정말 끝입니다! (고양이 사진도 가능합니다!)

![](/files/cute.png)

