import RPi.GPIO as GPIO
import time
import MySQLdb
import json
while 1:
    led=23 # led jie kou
    channel=18 #chuan gan qi jie kou
    data=[]
    j=0
    GPIO.setmode(GPIO.BCM)

    time.sleep(1)
    #kai shi liang deng
    GPIO.setup(led,GPIO.OUT)
    GPIO.output(led,GPIO.HIGH)
    #wen shi du jian ce
    GPIO.setup(channel,GPIO.OUT)
    GPIO.output(channel,GPIO.LOW)
    time.sleep(0.02)
    GPIO.output(channel,GPIO.HIGH)
    GPIO.setup(channel,GPIO.IN)
    while GPIO.input(channel)==GPIO.LOW:
        continue
    while GPIO.input(channel)==GPIO.HIGH:
        continue
    while j<40:
        k=0
        while GPIO.input(channel)==GPIO.LOW:
            continue
        while GPIO.input(channel)==GPIO.HIGH:
            k+=1
            if k>100:
                break
        
        if k<8:
            data.append(0)
        else:
            data.append(1)
        j+=1
    print("sensor is working.")

    #print (data)
    humidity_bit=data[0:8]
    humidity_point_bit=data[8:16]
    temperature_bit=data[16:24]
    temperature_point_bit=data[24:32]
    check_bit=data[32:40]

    humidity=0
    humidity_point=0
    temperature=0
    temperature_point=0
    check=0

    for i in range(8):
        humidity+=humidity_bit[i]*2**(7-i)
        humidity_point+=humidity_point_bit[i]*2**(7-i)
        temperature+=temperature_bit[i]*2**(7-i)
        temperature_point+=temperature_point_bit[i]*2**(7-i)
    
    tmp=humidity+humidity_point+temperature+temperature_point

    if check ==tmp:
        print ("temperature:",temperature,"humidity:",humidity)
    else:
       #print("wrong")
        print("temperature:",temperature,"humidity:",humidity,"check:",check,"tmp:",tmp)
    #shu ju chuan dao yun shu ju ku
    db=MySQLdb.connect(host='123.206.229.133',
                          port=3306,
                          user='root',
                          passwd='Caofeng2012@',
                          db='test')
    cur_db=db.cursor()
    value=[temperature,humidity,str(time.strftime('%X %Z',time.localtime(time.time())))]
    cur_db.execute("insert into dht(dht_tem,dht_hun,dht_time) values(%s,%s,%s)",value)
    db.commit()
    db.rollback()
    print ("success!!!")
    #fa song cheng gong  guan deng 
    GPIO.output(led,GPIO.LOW)
    
    time.sleep(5)#5s zhi xing yi ci
GPIO.cleanup()
                
