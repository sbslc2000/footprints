# BufferedReader와 BufferedWriter
버퍼를 사용하면 왜 빠른가?
[[BufferedReader를 사용해야하는 이유]]
## BufferedReader

### 한 줄 가져오기
```java
BufferedReader bf = new BufferedReader(new InputStreamReader(System.in));
String s = bf.readLine();
int i = Integer.parseInt(bf.readLine());
```

### 공백단위로 데이터 가공하기
```java
String s = bf.readLine();
StringTokenizer st = new StringTokenizer(s);
int a = Integer.parseInt(st.nextToken());
int b = Integer.parseInt(st.nextToken());

String array[] = s.split(" ");
```

## BufferedWriter
```java
BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
String s = "abcdefg";
bw.write(s+"\n");
bw.flush();
bw.close();
```
