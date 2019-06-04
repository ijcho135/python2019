# python2019
# 2018 항공기 주요제원 비교 및 분석

학과 | 학번 | 성명
---- | ---- | ---- 
항공우주공학과 |201627148| 조은주|


## 프로젝트 개요
항공기에 대한 주요제원을 알아보고, 여러 항공기를 제원에 대해 비교하는 그래프를 그려본다.
## 사용한 공공데이터 
[데이터보기](https://github.com/cybermin/python2019/blob/master/%EB%B6%80%EC%82%B0%EA%B5%90%ED%86%B5%EA%B3%B5%EC%82%AC_%EB%8F%84%EC%8B%9C%EC%B2%A0%EB%8F%84%EC%97%AD%EC%82%AC%EC%A0%95%EB%B3%B4_20190520.csv)

## 소스
* [링크로 소스 내용 보기](https://github.com/cybermin/python2019/blob/master/tes.py) 

* 코드 삽입
~~~python
import pandas as pf

dt18=pf.read_csv('한국공항공사_운송용_항공기_주요_제원_20180521.csv',encoding='CP949')
df18=list(dt18['기 종'])
dicname={} #{항공기기종:0~}
j=0
p=0
k=0
option_num=0;
dic_option={}  #{'option이름':'option_num'}
dic_option1={} #{'option이름':0~}

print("항공기 기종")
for item in df18:
    print(item) #항공기 기종을 표시
    dicname[item]=j
    j=j+1
name_main=input('항공기 기종을 입력하세요')
print("입력한 항공기:",name_main)
while(True):
    if name_main not in df18:
        name_main=input('항공기 기종을 다시 입력하세요')

    else:
        print("입력한 항공기:",name_main)
        break
for i in dt18.head():
    if i=='기 종':
        continue
    option_num=option_num+1
    print(str(option_num)+'.'+i)
    dic_option[str(option_num)]=i
    dic_option1[i]=k
    k=k+1
print(str(option_num+1)+'.다른 항공기와  비교')
dic_option[str(option_num+1)]='다른 항공기와 비교'
print(str(option_num+2)+'.항공기 재입력')
print("0. 종료")

while(True):

    if p!=0: #처음만 출력되지 않고 그 다음부터는 출력됨
        option_num=0
        #option종류 출력
        for i in dt18.head():
            if i=='기 종':
                continue
            option_num=option_num+1
            print(str(option_num)+'.'+i)
        print(str(option_num+1)+'.다른 항공기와  비교')
        dic_option[str(option_num+1)]='다른 항공기와 비교'
        print(str(option_num+2)+'.항공기 재입력')
        print("0. 종료")



    option=int(input("<option을 선택하세요>"))

    if option==0:
        print("종료되었습니다.")
        break # 0종료
    elif option==option_num+2:
         name_main=input('항공기 기종을 다시 입력하세요')
         while(True):
             if name_main not in df18:
                  name_main=input('항공기 기종을 다시 입력하세요')

             else:
                print("입력한 항공기:",name_main)
                break

    elif option>option_num+1 or option<0:
        print("option을 다시 선택하세요") #option number error
        continue

    else:
        name_compare_list=[name_main]

        import matplotlib.pyplot as plt
        from matplotlib import font_manager,rc

        font_name =font_manager.FontProperties(fname="c:/Windows/Fonts/malgun.ttf").get_name()
        rc('font',family=font_name)

        if option<=11:
            print("입력한 항공기:",name_main)
            result=dt18.iloc[dicname[name_main],dic_option1[dic_option[str(option)]]+1]
            print(dic_option[str(option)],':',result)

            if option==6:
                print("마하수(고도 10000m 일때):",result/1061.2) #고도 10000m 일때의 마하수
            continue

        elif option==12:
            for item in df18:
                print(item)
            print("입력한 항공기:",name_main)

            while(True): #비교할 항공기 입력받기

                name_compare=input("비교할 항공기를 입력하세요(0=항공기 입력 완료, q=옵션메뉴로 돌아가기):")

                if name_compare in name_compare_list:
                    print("이미 입력한 항공기")
                elif name_compare in dicname.keys():
                    print(name_compare)
                    name_compare_list.append(name_compare)
                elif name_compare=='0': break
                elif name_compare=='q':
                    break
                else: print("항공기 기종 입력 오류")
            if name_compare!='q':
                option_num=0
                #옵션내용
                for i in dt18.head():
                    if i=='기 종':
                     continue
                    option_num=option_num+1
                    print(str(option_num)+'.'+i)
                option_compare=int(input("비교할 제원을 선택하세요"))

                plt.xlabel('항공기')
                plt.ylabel(dic_option[str(option_compare)])

               # plt.title("항공기별",dic_option[str(option_compare)],"비교")
                y=[] #항공기에 해당하는 제원의 값을 list로
                y_sub=[]
                print(dic_option[str(option_compare)])
                for i in name_compare_list:
                    y1=dt18.iloc[dicname[i],dic_option1[dic_option[str(option_compare)]]+1]
                    #항공기 등급을 제외하고는 숫자로 인식하도록 하자
                    if option_compare<=10:
                        y.append(int(y1))
                        print(i,":",y1)

                    elif option_compare==11:
                        y.append(y1)
                        print(i,":",y1)





                plt.plot(name_compare_list,y)
                plt.show()

        print("입력한 항공기:",name_main)

    p=1

~~~