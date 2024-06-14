# PyMovie
A python study movie project 
參考 : https://github.com/guaosi/flask-movie
## 錯誤處理，若是找不到包名，進行以下的設定
2. 设置 PYTHONPATH
你可以在运行脚本之前设置 PYTHONPATH 环境变量，使得 Python 能找到你的 app 模块。在命令行中运行以下命令：

在 Windows 上：

sh
複製程式碼
set PYTHONPATH=C:\worker-projs\study-projs\PyMovie
在 Unix 或 MacOS 上：

sh
複製程式碼
export PYTHONPATH=/path/to/your/project
3. 使用相对导入
如果你的 models.py 文件和 __init__.py 文件在同一个包（app）中，你可以使用相对导入：

python
複製程式碼
# models.py
from datetime import datetime
from . import db
4. 确保以包的形式运行
确保你的应用是作为一个包来运行的。在你的项目根目录（PyMovie）下创建一个启动脚本 run.py，内容如下：

python
複製程式碼
# run.py
from app import app

if __name__ == "__main__":
    app.run()
然后在命令行中运行：

sh
複製程式碼
python run.py
这样 Python 会把 app 目录识别为一个包，从而能够正确地导入其中的模块。

5. 虚拟环境
确保你在一个虚拟环境中运行你的代码，并且虚拟环境已经激活：

sh
複製程式碼
python -m venv venv
source venv/bin/activate  # Unix or MacOS
venv\Scripts\activate     # Windows

# 安裝套件 (pip freeze 及 pip list 檢視套件)
pip install Flask
pip install flask_sqlalchemy  (https://pypi.org/project/Flask-SQLAlchemy/ 新版寫法有改)
pip install flask_script
pip install pymysql

# 建立專案目錄架構 (第一級結構在 PyMovie 下)
create manage.py  // 建立管理主程式
```python 
# coding:utf8
from app import app
from flask_script import Manager

manage = Manager(app)

if __name__ == "__main__":
    manage.run()
```
## 建立專案二級目錄架構 (第二級結構在 app 下)
mkdir app    // 將模組與檔案建於內
cd app
mkdir admin  // 後台管理模組 (在其底下建立 __init__.py)
mkdir home   // 前端模組 (在其底下建立 __init__.py)
mkdir static // 靜態目錄檔 (image,js,css...)
mkdir templates // html目錄檔
create __init__.py // 建立模組初始化程式
```python
# coding:utf8
from flask import Flask
from flask_sqlalchemy import SQLAlchemy
import pymysql

app = Flask(__name__)
app.config["SQLALCHEMY_DATABASE_URI"] = "mysql+pymysql://root:123456@127.0.0.1:3306/movie"  # 記得先在 mariadb 中建立 movie 資料庫
app.config["SQLALCHEMY_TRACK_MODIFICATIONS"] = True

app.debug = True
db = SQLAlchemy(app)

from app.home import home as home_blueprint
from app.admin import admin as admin_blueprint

app.register_blueprint(home_blueprint)
app.register_blueprint(admin_blueprint, url_prefix="/admin")
```
create models.py   // 建立資料庫模組程式 (python models.py 可生成資料表，及生成資料)
