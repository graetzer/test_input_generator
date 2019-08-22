
# Akumuli


docker run --name=influxdb -d -p 8181:8181 -p 8282:8282 -p 8383:8383 -p 4242:4242        -v /tmp/akumuli/data/:/Akumuli -e nvolumes=16 -e volume_size=1GB akumuli/akumuli:0.8.69-skylake

curl -XPOST "http://localhost:8181/api/search" --data '{"select":"cpu.user", "range":{"from": "20170101T000000", "to": "20170102T000000"}, "where":{"region":"ap-southeast-1"} }'


curl -XPOST "http://localhost:8181/api/query" --data '{"select":"cpu.user", "range":{"from": "20170101T000000", "to": "20170102T000000"}, "where":{"region":"ap-southeast-1"} }'

## Aggregate data

curl -XPOST "http://localhost:8181/api/query" --data '{"aggregate":{"cpu.user":"count"}, "range":{"from": "20170101T000000", "to": "20170102T000000"} }'
curl -XPOST "http://localhost:8181/api/query" --data '{"aggregate":{"cpu.user":"count"}, "range":{"from": "20170101T000000", "to": "20170102T000000"}, "pivot-by-tag":["OS"] }'

## Group Aggregation

Aggregation + downsampling

curl -XPOST "http://localhost:8181/api/query" --data '{"group-aggregate":{ "metric": "cpu.user", "step": "1h", "func": ["min", "max", "mean", "count"]}, "range":{"from": "20170101T000000", "to": "20170102T000000"} }'

## Group By Tags

pretty counterintuitive for some reason

curl -XPOST "http://localhost:8181/api/query" --data '{"group-aggregate":{ "metric": "cpu.user", "step": "1h", "func": ["min", "max", "mean", "count"]}, "range":{"from": "20170101T000000", "to": "20170102T000000"}, "pivot-by-tag": ["OS"] }'

curl -XPOST "http://localhost:8181/api/query" --data '{"group-aggregate":{ "metric": "cpu.user", "step": "1h", "func": ["min", "max", "mean", "count"]}, "range":{"from": "20170101T000000", "to": "20170102T000000"}, "pivot-by-tag": ["instance-type"] }'


curl -XPOST "http://localhost:8181/api/query" --data '{"group-aggregate":{ "metric": "cpu.user", "step": "1h", "func": ["min", "mean", "max"]}, "range":{"from": "20170101T220000", "to": "20170102T000000"}, "pivot-by-tag": ["OS", "instance-type"] }'

# InfluxDb

docker run --name=influxdb -d -p 8086:8086 -v $PWD:/var/lib/influxdb influxdb
docker exec -it influxdb influx

docker exec -it influxdb influx -type=flux -execute '<flux query>'

dataSet
  |> filter(fn: (r) =>
     r.device_id == "demo001217"
  )
  |> window(every: 5m)
  |> group(columns: ["model", "manufacturer"])
  |> mean()
