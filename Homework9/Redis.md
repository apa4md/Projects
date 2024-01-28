# Redis
## Вставка данных производилась через модуль redis pythonimport json
import redis
r = redis.StrictRedis(host='localhost', port=6379, db=1)
with open('large-file.json') as data_file:
        test_data = json.load(data_file)
###for zset
for k in range(len(test_data)):
    actor=test_data[k]['actor']['login']
    d=test_data[k]
    f=float(k)
    r.zadd(actor,{d['type']:f})

####for hset
for k in range (0,len(test_data)):
    d=test_data[k]
    r.hset(k,d['id'],str(d))

##### for list
for i in test_data:
    d=json.dumps(i)
    r.lpush(i['id'],d)

#### fos string
for i in test_data:
    d=str(i)
    r.set(i['id'],d)

##### время вставки и скорость чтения
###### zset
####### запись
real    0m1.883s
user    0m0.955s
sys     0m0.370s
####### чтение 
6 microseconds

###### string
####### запись
real    0m2.304s
user    0m1.353s
sys     0m0.338s
####### чтение 
5 microseconds

###### hset
####### запись
real    0m2.140s
user    0m1.177s
sys     0m0.353s
####### чтение
9 microseconds

###### list
####### запись
real    0m2.075s
user    0m1.183s
sys     0m0.377s
####### чтение
25 microseconds
