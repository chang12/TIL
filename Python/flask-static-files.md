npm 으로 다운로드 받은 패키지들을 HTML 페이지에서 사용하기 위해서는 Flask 백엔드에서 서빙해줘야 한다.

```
project/
    node_modules/
    static/
    templates/
    app.py
    package.json
```

```python
# static_url_path 값을 추가해줘야 한다.
app = Flask(__name__, static_url_path='')


# template 파일은 별도의 설정없이 인식해준다.
@app.route('/')
def hello_world():
    return render_template('index.html')


@app.route('/node_modules/<path:path>')
def serve_npm_packages(path):
    return send_from_directory('node_modules', path)


@app.route('/static/<path:path>')
def serve_static_files(path):
    return send_from_directory('static', path)
```

이 정도로 기본 세팅을 갖춰놓고, node_modules / static / templates 하위에서는 서브 디렉토리를 생성해서 분간하면 어떨까 생각해봤다.