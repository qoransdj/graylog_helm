헬름으로 설치 후에

kubectl get pods -n [namespace]로 graylog pod 확인

kubectl logs -n [namespace] [graylog pod] 로 해시가 포함된 초기 접속 url 획득

url 상 0.0.0.0:9000 을  localhost:30900 으로 수정 후 진입


목록 조회
curl -u admin:test123! \
-H "Accept: application/json" \
http://localhost:30900/api/streams | jq

curl -u admin:test123! \
-H "Accept: application/json" \
http://localhost:30900/api/system/indices/index_sets | jq


스트림 생성
 curl -X POST \
     -u "admin:test123!" \
     -H "X-Requested-By: CLI-Script" \
     -H "Content-Type: application/json" \
     -H "Accept: application/json" \
     -d '{
       "entity": {
         "title": "Application Production Logssssss",
         "description": "Stream for production app log filtering",
         "index_set_id": "6a7156a4b5265f5edc406ed3",
         "matching_type": "AND",
         "remove_matches_from_default_stream": false
       }
     }' \
     "http://localhost:30900/api/streams"

---------------------------------------------------------------
INDEX_ID=$(curl -s \
-u admin:test123! \
http://localhost:30900/api/system/indices/index_sets \
| jq -r '.index_sets[] 
| select(.title=="index_test") 
| .id')

echo $INDEX_ID
-------------------------------------------------------------------------
curl -X POST \
-u "admin:test123!" \
-H "X-Requested-By: CLI-Script" \
-H "Content-Type: application/json" \
-H "Accept: application/json" \
-d "{
  \"entity\": {
    \"title\": \"Application Production Logs\",
    \"description\": \"Stream for production app log filtering\",
    \"index_set_id\": \"$INDEX_ID\",
    \"matching_type\": \"AND\",
    \"remove_matches_from_default_stream\": false
  }
}" \
http://localhost:30900/api/streams