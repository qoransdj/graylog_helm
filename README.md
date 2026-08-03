헬름으로 설치 후에

kubectl get pods -n [namespace]로 graylog pod 확인

kubectl logs -n [namespace] [graylog pod] 로 해시가 포함된 초기 접속 url 획득

url 상 0.0.0.0:9000 을  localhost:30900 으로 수정 후 진입
