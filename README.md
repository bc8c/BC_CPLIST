# 1. 개발환경 세팅
### 1.1. 사용중인 도커 삭제 및 네트워크 초기화

```bash
docker rm -f $(docker ps –aq)
docker network prune
```
수행중에 나타나는 메시지에서 `Y`를 선택한다.  


# 2. 클라이언트 설치 및 네트워크 시작
### 2.1. fabric dependencies 설치
```bash
npm install fabric-ca-client fabric-client fabric-network
```
### 2.2. fabric 네트워크 실행 (fabcar)
경로 : /home/bstudent/fabric-samples/fabcar
```bash
./startFabric.sh
```
### 2.3. CA 로그 출력하기
```bash
docker logs –f ca.example.com
```


# 3. 사용자 등록하기
### 3.1. Admin 등록하기
경로 : /home/bstudent/fabric-samples/fabcar
```bash
node javascript/enrollAdmin.js
```
실행 경로에 `wallet` 폴더가 생성되며 내부에 `admin` 폴더와 함께 key가 생성됨

### 3.2. User1 등록하기
경로 : /home/bstudent/fabric-samples/fabcar
```bash
node javascript/registerUser.js
```
`wallet` 폴더에 `user1` 폴더와 함께 key가 생성됨



# 4. Query 사용 하기
### 4.1. query.js 실행하기
경로 : /home/bstudent/fabric-samples/fabcar
```bash
node javascript/query.js
```
key를 `CAR0`부터 `CAR9`까지 갖는 데이터를 출력하게됨



### 4.2. query.js 수정하기
경로 : /home/bstudent/fabric-samples/fabcar
```bash
gedit javascript/query.js
```
`query.js` 파일을 수정할수 있도록 에디터로 실행함  
main 함수의 `queryAllCars` 함수 호출 부분을 아래 `queryCar` 호출코드로 변경후 저장  
(`[1]_queryCar4.txt` 파일 참고 )
##### 수정전
```javascript
const result = await contract.evaluateTransaction('queryAllCars');
```
##### 수정후
```javascript
const result = await contract.evaluateTransaction('queryCar', 'CAR4');
```
`query.js` 실행

```bash
node javascript/query.js
```
`key`가 `CAR4`인 데이터만 출력하게됨



# 5. 원장 업데이트 하기
### 5.1. invoke.js 실행하기
경로 : /home/bstudent/fabric-samples/fabcar
```bash
node javascript/invoke.js
```
`key`를 `CAR12`로 갖는 데이터를 생성함 :
```
{"Key":"CAR12", "Record":{"colour":"Black","make":"Honda","model":"Accord","owner":"Tom"}
```


### 5.2. query.js 로 결과 확인하기
경로 : /home/bstudent/fabric-samples/fabcar
```bash
gedit javascript/query.js
```
`query.js` 파일을 수정할수 있도록 에디터로 실행함


main 함수의 `queryCar` 함수 호출 부분을 아래 `queryCar` 호출코드로 변경후 저장  
(`[2]_queryCar12.txt` 파일 참고 )
##### 수정전
```javascript
const result = await contract.evaluateTransaction('queryCar', 'CAR4');
```
##### 수정후
```javascript
const result = await contract.evaluateTransaction('queryCar', 'CAR12');
```
`query.js` 실행

```bash
node javascript/query.js
```
`key`가 `CAR12`인 데이터만 출력하게됨


### 5.3. Car Owner 변경하기
경로 : /home/bstudent/fabric-samples/fabcar
```bash
gedit javascript/invoke.js
```
invoke.js 파일을 수정할수 있도록 에디터로 실행함



main 함수의 `createCar` 함수 호출 부분을 아래 `changeCarOwner` 호출코드로 변경후 저장  
(`[3]_changeCar12Owner.txt` 파일 참고 )

##### 수정전
```javascript
await contract.submitTransaction('createCar', 'CAR12', 'Honda', 'Accord', 'Black', 'Tom');
```
##### 수정후
```javascript
const result = await contract.submintTransaction('changeCarOwner', 'CAR12', 'Dave');
```
`query.js` 실행

```bash
node javascript/invoke.js
```
`key`가 `CAR12`인 데이터의 `Owner`를 `Dave`로 변경
