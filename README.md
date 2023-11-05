# A Docker image with the [Coturn TURN server] (https://github.com/coturn/coturn)

## 원본 블로그 링크
클라우드 배포를 위한 TURN 서버 조정  
: Azure 의 자원을 활용하여 배포 자동화를 함
> 우리는 부하 분산된 TURN 서버 세트에 대한 호스팅 환경으로 Azure를 선택했으며 Ubuntu VM , Azure Database for PostreSQL , Azure KeyVault 및  Azure Virtual Network 와 같은 다양한 서비스를 사용했습니다 . Azure에 대한 배포 프로세스를 자동화하기 위해 우리는  10분 이내에 프로덕션 준비가 완료된 부하 분산된 TURN 서버를 설정할 수 있는 스크립트와 ARM 템플릿을 개발했습니다.

- https://devblogs.microsoft.com/ise/2018/01/29/orchestrating-turn-servers-cloud-deployment/

## Usage

1. Put certificate to /etc/ssl/certs/turn
2. Create Postgres DB and add user or shared secret depending on authentication method: long term credentials (long_term_creds) or temporary passwords based on shared secret (shared_secret).
For more information see https://github.com/anastasiia-zolochevska/azturnlb and https://github.com/coturn/coturn
3. Run 
```bash
sudo docker run -v /etc/ssl/certs/turn:/etc/ssl -d -p 3478:3478 -p 3478:3478/udp -p 5349:5349 -p 5349:5349/udp --restart=always zolochevska/3dsrelay "<PG_CONNECTION_STRING>" <REALM> <auth_method>
```

Example:
```bash
sudo docker run -v /etc/ssl/certs/turn:/etc/ssl -d -p 3478:3478 -p 3478:3478/udp -p 5349:5349 -p 5349:5349/udp --restart=always zolochevska/3dsrelay "host=something.postgres.database.azure.com dbname=coturndb user=coturn@something password=ADecentPassword? connect_timeout=30 sslmode=require port=5432" azturntst.org shared_secret
```
