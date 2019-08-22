

curl -XPOST "http://localhost:8181/api/search" --data '{"select":"cpu.user", "range":{"from": "20170101T000000", "to": "20170102T000000"}, "where":{"region":"ap-southeast-1"} }'


curl -XPOST "http://localhost:8181/api/query" --data '{"select":"cpu.user", "range":{"from": "20170101T000000", "to": "20170102T000000"}, "where":{"region":"ap-southeast-1"} }'

curl -XPOST "http://localhost:8181/api/query" --data '{"aggregate":{"cpu.user":"count"}, "range":{"from": "20170101T000000", "to": "20170102T000000"} }'