# 플라스크 GET,POST,DELETE 이용해서 간단 CRUD구현

```python
import json
from flask import Flask,jsonify,request

app = Flask(__name__)

@app.route('/')
def hello_flask():
    return 'Hello Flask!'

if __name__ == '__main__':
    app.run()

# 데이터베이스
weapon = []
name_db = {}
name_id = 0

@app.route('/weapon')
def read_weapon():
    return jsonify({'weapon list':weapon})

# /add_weapon?name=<weapon_name>&cnt=<weapon_count>
@app.route('/add_weapon',methods = ['POST'])
def add_weapon():
    global name_id # 전역변수를 불러온다
    # 주소창에서 받은 'name'값을 대입하고, 디폴트값은 None, 타입은 문자열
    name = request.args.get('name',default=None,type=str)
    cnt = request.args.get('cnt',default=None,type=int)
    # 입력이 없을 때
    if not name or not cnt:
        return '무기 생성에 실패했습니다. name과 cnt(수량)을 확인하세요.'
    
    # 이미 존재하는 무기일 때
    elif name in name_db:
        return '이미 존재하는 무기입니다. 업데이트를 해주세요'
    
    else:
        # 무기를 정보를 추가한다
        weapon.append({'name':name,'count':cnt})
        # 이름과 id를 저장
        name_db[name] = name_id
        name_id+=1
        return '무기를 생성했습니다'

# /add_weapon?name=<weapon_name>
@app.route('/update_weapon',methods = ['POST'])
def update_weapon():
    name = request.args.get('name',default=None,type=str)
    # 입력이 없을 시, 입력한 무기가 데이터베이스에 없을 시
    if not name or name not in name_db:
        return '무기정보 업데이트에 실패했습니다. name을 정확히 입력하세요.'
    else:
        data = request.get_json()
        rename = data['rename']
        cnt = data['cnt']
        weapon[name_db[name]]['name'] = rename
        weapon[name_db[name]]['count'] = cnt
        return '무기정보를 업데이트 했습니다.'
    
# /delete_weapon?name=<weapon_name>
@app.route('/delete_weapon',methods = ['DELETE'])
def delete_weapon():
    del_name = request.args.get('name',default=None,type=str)
    # 입력이 없거나, 입력받은 무기가 없을 경우
    if not del_name or del_name not in name_db:
        return '무기 삭제에 실패했습니다. 이름을 정확히 입력해주세요'
    else:
        # 해당 무기의 정보를 None으로 바꾼다
        weapon[name_db[del_name]] = None
        # 무기이름 데이터베이스에서 삭제한다
        del name_db[del_name]
        return '무기를 삭제했습니다'
        

```





- GET : 웹페이지의 자원을 가져온다
- POST : 웹페이지의 자원을 추가한다.
- DELETE : 웹페이지의 자원을 삭제한다



- url PArameter
  - ? : 다음에 GET할 변수를 쓴다
  - & : '그리고' 역할

