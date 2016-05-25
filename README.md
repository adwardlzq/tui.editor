## 설치

리포의 dist 디렉토리의 파일들을 다운로드 받아서 사용하실수있습니다.
이경우 bower.json의 디펜던시 라이브러리들을 따로 받으셔야합니다.

bower를 이용해서 설치하는것을 추천합니다.
[릴리즈노트](https://github.com/shiren/tui-editor/releases)를 참고하여 #뒤에 버전을 지정해서 설치합니다.
예제는 0.0.7버전의 설치예제입니다.

```
bower install git@github.com:shiren/tui-editor.git#0.0.7
```

이후 업데이트는 아래와같이 할수있습니다.

```
bower update
```

## 임시 설치 가이드

현재 에디터가 개발중에 있습니다. 미리 에디터를 적용하기 위한 임시 배포입니다.
에디터를 정상적으로 사용하기 위해서는 하단에 샘플 코드와같이 디펜던시 js, css 파일들과 에디터js, css파일이 로드가 되어야합니다.
tui-editor.css는 에디터에 필요한 css스타일들이고
tui-editor-contents.css는 wysiwyg에디터나 마크다운 preview에서 보여질 컨텐트의 스타일들입니다.
tui-editor-contents.css는 기호에 맞게 수정하실수 있으며 에디터를 통해 만들어진 컨텐츠를 보여줄때 같은 내용을 사용하실수 있습니다.(css파일 내용 참고)
아래 예제를 확인 바랍니다.

``` html
<!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title>DEMO</title>
    <script src="bower_components/jquery/dist/jquery.js"></script>
    <script src="bower_components/tui-code-snippet/code-snippet.js"></script>
    <script src="bower_components/marked/lib/marked.js"></script>
    <script src="bower_components/toMark/dist/toMark.js"></script>
    <script src="bower_components/codemirror/lib/codemirror.js"></script>
    <script src="bower_components/highlightjs/highlight.pack.js"></script>
    <script src="bower_components/Squire/build/squire-raw.js"></script>
    <script src="bower_components/tui-editor/dist/tui-editor.min.js"></script>
    <link rel="stylesheet" href="bower_components/codemirror/lib/codemirror.css">
    <link rel="stylesheet" href="bower_components/highlightjs/styles/github.css">
    <link rel="stylesheet" href="bower_components/tui-editor/dist/tui-editor.css">
    <link rel="stylesheet" href="bower_components/tui-editor/dist/tui-editor-contents.css">
</head>
<body>
<div id="editSection"></div>
<script>
    $('#editSection').tuiEditor({
        initialEditType: 'markdown',
        previewStyle: 'vertical',
        height: 300,
        events: {
            'load': function() {
                console.log('handler');
            }
        }
    });
</script>
</body>
</html>
```

## 옵션 설명

* viewOnly: {boolean}, true가 넘겨지면 렌더링만 가능한 뷰모드로 에디터를 생성합니다. 마크다운을 html로 렌더링합니다.
* initialEditType: 'markdown'과 'wysiwyg'둘중 하나를 선택해서 에디터를 시작합니다.
* previewStyle: 마크다운의 경우 preview pane과 edit pane을 2단으로 보여줄지 tab형식으로 보여줄지를 정하는 옵션입니다.(tab, vertical)
* height: 에디팅영역의 기본 높이를 결정합니다.(숫자, "auto"), "auto"입력시 컨텐츠에 따라서 에디터가 늘어납니다.
* events: 내부 이벤트에 대응하는 핸들러를 셋팅합니다.
    * 이벤트 목록은 하단 API파트에 있습니다.
* exts: 사용할 익스텐션들을 배열로 지정합니다.
    * `exts: ['scrollFollow']`
    * scrollFollow: 마크다운 에디터에서 프리뷰영역이 에디팅 영역의 스크롤을 따라갑니다..
    * colorSyntax: 컬러를 사용할수 있게합니다.
    * mark: 텍스트 마커를 삽입합니다.(API목록 하단에 설명)
* hooks: 이미지서버와의 연동등을 처리하는 훅을 바인드합니다.
    * addImageBlobHook : 파일 blob을 이용해 이미지를 서버에 업로드합니다.

## 이미지 서버 연동
addImageBlobHook을 이용해서 이미지 파일을 blob으로 전달받아서 연동합니다.
블롭을 통한 이미지 서버 연동은 [여기](https://developer.mozilla.org/en/docs/Using_files_from_web_applications)의 Handling the upload process for a file파트를 참조해주세요


``` javascript
$('#editSection').tuiEditor({
    initialEditType: 'markdown',
    previewStyle: 'tab',
    height: 300,
    hooks: {
        'addImageBlobHook': function(blob, callback) {
            //이미지 블롭을 이용해 서버 연동 후 콜백실행
            //callback('이미지URL');
        }
    }
});
```

## API

``` javascript
//아래와 같이 일반적인 jQuery 플러그인 인터페이스를 이용합니다.

$('#editSection').tuiEditor('focus');

var content = $("#editSection").tuiEditor("getValue");

$("#editSection").tuiEditor("setValue", "# Hello!!");
```

* focus: 에디터에 포커스를 줍니다.
* hide: 에디터를 숨깁니다.
* show: 에디터를 보입니다.
* getValue: 입력된 마크다운 컨텐트를 가져옵니다.
* setValue: 에디터에 마크다운 컨텐트를 셋팅합니다.
* changeMode: 에디터의 타입을 변경한다(인자로는 wysiwyg과 markdown)
* contentHeight: 에디터의 컨텐트 영역의 높이값을 인자로 넘겨 지정하거나 현재의 높이값을 반환합니다
* moveCursorToEnd: 에디터 컨텐츠의 맨끝으로 스크롤및 커서이동한다.
* moveCursorToStart: 에디터 컨텐츠의 맨 처음으로 스크롤및 커서이동한다.
* on: 이벤트핸들러와 핸들러평션을 파라메터로 넘겨 에디터 내부이벤트를 바인드 할수있습니다.
    * jQuery의 이벤트 네임스페이스와 동일하게 적용할수 있어 추후 특정 이벤트핸들러만 삭제할수있습니다(ex. change.dooray)
    * event목록
        * load: 에디터가 정상적으로 설치된후 발생
        * change: 에디터의 컨텐츠가 변경되면 발생
        * changeMode: 에디터 모드가 변경되는 발생
        * stateChange: 커서상의 위치에 해당하는 컨텐츠의 종류가 변경되면 발생(bold, italic)
        * focus
        * blur
        * show
        * hide
        * keyMap: 에디터상에서 입력되는 키조합관련 이벤트를 발생한다.('SHIFT+CTRL+A' 이런식의 키입력 데이터를 얻을수 있다)
* off : 이벤트 핸들러를 제거합니다. 네임스페이스로 특정 이벤트 핸들러만 삭제할수있습니다
    * "change"와 실행시 모든 change이벤트 제거 "change.dooray"는 dooray라는 네임스페이스가 있는 change이벤트만 제거 ".dooray"는 모든 이벤트의 ".dooray"네임스페이스를 가진 핸들러제거

``` javascript
$('#editSection').tuiEditor('on', 'load', handler);
$('#editSection').tuiEditor('on', 'load.dooray', handler);
$('#editSection').tuiEditor('off', '.dooray', handler);
$('#editSection').tuiEditor('off', 'load.dooray', handler);
```

## Extension API

### mark
텍스트 셀렉션 기준의 마커를 추가하거나 관리하여 사용자 UI와 연동할 수 있습니다..
demo-marker.html과 demo-viewOnlyMarker.html를 참조하세요.

* setValueWithMarkers(마크다운, 마커데이터): 에디터에 텍스트를 셋팅하면서 해당컨텍스트의 마커데이터를 복구합니다. 마커데이터는 exportMarkers로 얻어진 데이터입니다.

``` javascript
$('#editSection').tuiEditor('setValueWithMarkers', '# TEXT', 마커데이터);
```

* addMarker(아이디): 에디터의 현재 셀렉션에 마커를 삽입합니다. 마커를 구별할 아이디를 입력합니다, 마커의 정보가 담긴 객체가 리턴됩니다.

``` javascript
var marker = $('#editSection').tuiEditor('addMarker', 'm1');
```

* selectMarker(아이디): 마커의 셀렉션을 생성합니다.
* clearSelect(): 셀렉션을 취소합니다.
* exportMarkers(): 현 컨텐츠의 마커정보를 저장하기위해 모든 마커의 JSON데이터를 받습니다
* getMarker(아이디): 특정 아이디의 마커 정보를 얻습니다.
* getMarkersAll(): 모든 마커정보를 얻습니다.
* removeMarker(아이디): 마커를 삭제합니다., 삭제된 마커정보가 리턴됩니다
* 이벤트
    * markerUpdated: 마커의 위치가 업데이트되거나 추가되면 이벤트가 발생합니다. 인자로 모든 마커정보가 넘어갑니다

## 커스텀 렌더러의 적용
marked를 확장해 추가적인 마크다운을 삽입하고자 한다면 tui.Editor.markedRenderer를 이용해서 확장하세요.
에디터에서 사용하는 렌더러입니다. 덮어쓰지 말고 확장해서 사용해주세요

## 버전별 업데이트 유의점
* 0.5.0
    * 위지윅에디터에서 iframe이 제거되었습니다.
    * contentCSSStyles옵션이 더이상 필요없어졌습니다.(deprecated)
* 0.4.0
    * viewOnly 옵션이 추가되었습니다. true를 넘기면 html 렌더링만 가능한 뷰모드로 에디터를 생성합니다.
* 0.0.9
    * CSS 클래스명 대거 수정(에디터 디자인 커스터마이징시 유의)
    src/css/tui-editor.css 변경점 참고
* 0.0.8
    * addImageFileHook 삭제
* 0.0.7
    * 네이밍 변경
        * 리포지토리 경로
        * jQuery API명
        * 파일 경로
* 0.0.6
    * 디펜던시 모듈 code-snippets의 네이밍이 변경됨에 따라 사용 경로도 바뀌었습니다. tui-code-snippets의 경로를 확인바랍니다.
* 0.0.2
    * 에디터 셋팅한뒤 바로 에디터에 접근할때 iframe이 셋팅되지 않아 문제가될수있습니다.
    iframe의 셋팅이 비동기로 이루어집니다. 에디터를 생성한시점에 에디터 API를 이용한다던지 에디터에 접근할때는 load이벤트를 이용해주시기바랍니다.
    * 기존에 디펜던시 모듈들이 모두포함된 full버전이 제거되었습니다 따라서 디펜던시 모듈들을 직접 로드해야합니다(샘플 참조)
