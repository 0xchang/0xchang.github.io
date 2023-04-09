---
title: rsa合集
date: 2023-03-28 21:23:03
tags:
- rsa
- crypto
---

### gcd(a,b)
```python
def gcd(a:int,b:int):
	# a%b=c
	# b%c=d
	# 当余数为0，除数就是最大公约数
	while b:
		a,b = b,a%b
	return a
```
### 互素
```python
def prime(a,b):
	# a和b的最大公约数为1
	if gcd(a,b)==1:
		return True
	else:
		return False
```
### Euler(n)
```python
def Euler(n):
	# 找到所有小于n且和n互素的数的个数
	count=0
	for i in range(1,n):
		if prime(i,n):
			count+=1
	return count
```
### Exgcd(a,b)
* ax + by = gcd(a,b)
* 求存在整数x,y
* 对于a>b,当b=0,gcd(a,b)=a;x=1,y=0
* 设ax1+by1=gcd(a,b)
* 有bx2+(a%b)y2=gcd(b,a%b)
* 有gcd(a,b)=gcd(b,a%b)
* 则ax1+by1=bx2+(a%b)y2
* ax1+by1=bx2+(a-\[a/b\]\*b)\*y2=ay2+bx2-\[a/b\]\*by2
* 即ax1+by1 == ay2+b(x2-\[a/b\]\*y2)
* x1=y2, y1=x2-\[a/b\]\*y2
* x1,y1的值根据x2,y2求出
* 递归定义，b总有为0的时候
```python
def exgcd(a,b):
	if b==0:
		return a,1,0
	else:
		d,x,y=exgcd(b,a%b)
		x,y=y,x-(a//b)*y
		return d,x,y
```
### mod_invert(a,b)
```python
def mod_invert(a,b):
	# 求模逆
	d,x,y=exgcd(a,b)
	# 判断是否互质
	if d!=1:
		print('no')
	else:
		print((x%b+b)%b)
```
#### 中国剩余定理
* 有x≡x1 mod n1
* x≡x2 mod n2
* x≡x3 mod n3...
* 求解所有模数的乘积N=n1\*n2\*n3...
* 对于每个i计算ni'=N//ni,求解m\*ni'≡1(mod ni)的一个解m，m和ni'互质，用扩展欧几里得算法求解
* 计算x=求和(xi\*ni'\*m)
```python
import gmpy2
from funtools import reduce
def CRT(items):
	N=reduce(lamda: x,y:x*y, (i[1] for i in items))
	result=0
	for x,n in items:
		ni=N//n
		d,r,s=gmpy2.gcdext(n,ni)
		if d!=1:
			raise "gcd error"
		result+=x*ni*s
	return result%N, N
```
### RSA已知p,q,e求解D
#### 原理
* φ(pq)=(p-1)\*(q-1)
* e\*d ≡1 mod φ(pq) 求解d
#### 题目
```python
在一次RSA密钥对生成中，假设p=473398607161，q=4511491，e=17
求解出d作为flag提交
```
#### 题解
```python
import gmpy2
p=473398607161
q=4511491
e=17
phi_n=(p-1)*(q-1)
d=gmpy2.invert(e,phi_n)
print(d)
```
### RSA已知p,q,e,c求解明文
* c=m^e(mod n)
* m=c^d(mod n)
#### 题目
```python
Math is cool! Use the RSA algorithm to decode the secret message, c, p, q, and e are parameters for the RSA algorithm.

p =  9648423029010515676590551740010426534945737639235739800643989352039852507298491399561035009163427050370107570733633350911691280297777160200625281665378483
q =  11874843837980297032092405848653656852760910154543380907650040190704283358909208578251063047732443992230647903887510065547947313543299303261986053486569407
e =  65537
c =  83208298995174604174773590298203639360540024871256126892889661345742403314929861939100492666605647316646576486526217457006376842280869728581726746401583705899941768214138742259689334840735633553053887641847651173776251820293087212885670180367406807406765923638973161375817392737747832762751690104423869019034

Use RSA to find the secret message
```
#### 题解
```python
import gmpy2

# p=
# q=
# e=
# c=

n=p*q
phi_n=(p-1)*(q-1)
d=gmpy2.invert(e,phi_n)
m=pow(c,d,n)
print(m)
```
### RSA已知q,p,dp,dq,c求解明文
* dp=d(mod p-1)
* dq=d(mod q-1)
* n=p\*q
* m1=c^dp(mod p)
* m2=c^dq(mod q)
* invp=gmpy2.invert(p,q)
* (m=((m2-m1)\*invp%q)+m1)(mod n)
#### 题目
```python
p = 8637633767257008567099653486541091171320491509433615447539162437911244175885667806398411790524083553445158113502227745206205327690939504032994699902053229 
q = 12640674973996472769176047937170883420927050821480010581593137135372473880595613737337630629752577346147039284030082593490776630572584959954205336880228469 
dp = 6500795702216834621109042351193261530650043841056252930930949663358625016881832840728066026150264693076109354874099841380454881716097778307268116910582929 
dq = 783472263673553449019532580386470672380574033551303889137911760438881683674556098098256795673512201963002175438762767516968043599582527539160811120550041 
c = 24722305403887382073567316467649080662631552905960229399079107995602154418176056335800638887527614164073530437657085079676157350205351945222989351316076486573599576041978339872265925062764318536089007310270278526159678937431903862892400747915525118983959970607934142974736675784325993445942031372107342103852
```
#### 题解
```python
import gmpy2
import libnum

# p = 
# q = 
# dp = 
# dq = 
# c = 

# gmpy2.invert返回mpz类型转为int型
invq=int(gmpy2.invert(p,q))
mp=pow(c,dp,p)
mq=pow(c,dq,q)
m=((mp-mq)*invq%p)*q+mq
print(libnum.n2s(m))
```
#### dp泄露，已知e,n,c,dp
* dp = d (mod p-1)
* n=p\*q
* m = c^d (mod n)
* e * dp = e * d(mod p-1)
* e * d = k2(p-1) + e * dp
* d * e = k1(p-1) * (q-1) +1
* k2(p-1) + e * dp = k1(p-1) * (q-1) +1
* (p-1) *  (k1 * (q-1) -k2 )+1=e * dp
* 设i=k1 * (q-1) -k2 
*  (p-1)i + 1 = dp * e
* 1< i < e
```python
# e=
# n=
# dp=
# c=

import gmpy2
from Crypto.Util.number import long_to_bytes

for i in range(1,e):                   #在范围(1,e)之间进行遍历
    if(dp*e-1)%i == 0:
        if n%(((dp*e-1)//i)+1) == 0:   #存在p，使得n能被p整除
            p=((dp*e-1)//i)+1
            q=n//p
            phi=(q-1)*(p-1)            #欧拉定理
            d=gmpy2.invert(e,phi)         #求模逆
            m=pow(c,d,n)               #快速求幂取模运算           
            print(long_to_bytes(m))
```
#### 共模攻击，已知c1,c2,e1,e2,n
* 适用情况：明文m、模数n相同，公钥指数e、密文c不同，gcd(e1,e2)=1也就是e1和e2互质。
```python
# c1=
# c2=
# e1=
# e2=
# n=

import gmpy2
from Crypto.Util.number import long_to_bytes

assert gmpy2.gcd(e1,e2)==1
_, r, s = gmpy2.gcdext(e1, e2)

m = pow(c1, r, n) * pow(c2, s, n) % n
print (long_to_bytes(m))
```
#### rsa公钥读取
```python
import gmpy2
from Crypto.Util.number import *
from Crypto.PublicKey import RSA
# 从公钥里面提取n 和 e
with open('./pub.key','r') as f:
    key = RSA.import_key(f.read())
e = key.e
n = key.n
#通过分解n得到p和q
#p = 
#q = 
d = gmpy2.invert(e,(p-1)*(q-1))
with open('./flag.enc','rb')as f:
    c = bytes_to_long(f.read())
    m = pow(c,d,n)
    print(long_to_bytes(m))
```
#### rsaroll
```python
import libnum
from Crypto.Util.number import long_to_bytes
import gmpy2
# list1=[1123,123,123,132]
flag=""
n=920139713
q=18443
p=49891
e=19
for i in list1:
	c=i
	d = gmpy2.invert(e,(p-1)*(q-1))
	m = pow(c, d, n) 
	string = long_to_bytes(m)
	flag+=string.decode()
print(flag)
```
#### 低加密指数攻击
##### 普通
* e很小，n很大
* 设e=3，m^3 = c (mod n)
* c = m^3(mod n)
* c^(-3) = m(mod n)
```python
import gmpy2
import os
from functools import reduce 
from Crypto.Util.number import long_to_bytes 
# gmpy2.crt(c,n)
def CRT(items):
    N = reduce(lambda x, y: x * y, (i[1] for i in items))
    result = 0
    for a, n in items:
        m = N // n
        d, r, s = gmpy2.gcdext(n, m)
        if d != 1:
            raise Exception("Input not pairwise co-prime")
        result += a * s * m
    return result % N, N
# e, n, c
# e = 0x3
# n=[]
# c=[]
data = list(zip(c, n))
x,n = CRT(data)
m = int(gmpy2.iroot(x,e)[0])
print(long_to_bytes(m))
```
* 利用二分法逼近明文数值
```python
from math import isqrt, gcd
from Crypto.Util.number import long_to_bytes

def rsa_low_exponent_attack(n, e, c):
    # 计算n的平方根
    sqrt_n = isqrt(n)

    # 使用二分搜索逐步逼近m的值
    low = 0
    high = n
    while low <= high:
        mid = (low + high) // 2

        # 计算mid ^ e % n
        result = pow(mid, e, n)

        # 如果result和c的值一样，说明mid是m的一个候选值
        if result == c:
            return mid

        # 如果result比c的值小，则将搜索范围移到[mid+1, high]
        elif result < c:
            low = mid + 1

        # 如果result比c的值大，则将搜索范围移到[low, mid-1]
        else:
            high = mid - 1

    # 如果搜索失败，则返回None
    return None


# 测试
if __name__ == "__main__":
	# n=
	# e= 
	# c=
	m = rsa_low_exponent_attack(n, e, c)
	# 通过整数转换成明文并打印出来
	print(long_to_bytes(m))

```
##### 爆破
* 爆破e，给出多组c和n
```python
import gmpy2
import os
from functools import reduce 
from Crypto.Util.number import long_to_bytes 
import numpy as np
# 生成15个素数
primes = np.array([2])
p = 3
while len(primes) < 15:
	if np.all(p % primes != 0):
		primes = np.append(primes, p)
	p += 2  # 步长为2
def CRT(items):
	N = reduce(lambda x, y: x * y, (i[1] for i in items))
	result = 0
	for a, n in items:
		m = N // n
		d, r, s = gmpy2.gcdext(n, m)
		if d != 1:
			raise Exception("Input not pairwise co-prime")
		result += a * s * m
	return result % N, N
# n1 = 
# c1 = 
# n2 = 
# c2 =  
# n3 = 
# c3 = 
# c=[c1,c2,c3]
# n=[n1,n2,n3]
data = list(zip(c, n))
x,n = CRT(data)
for i in primes:
	e = int(i)
	m = int(gmpy2.iroot(x, e)[0])
	print(long_to_bytes(m))
```
##### 公因数求解d,p
* 当有多个n和c时，求解ni和nj的公因数d，当d!=1，ni=d\*p，分解成功
```python
import gmpy2
import libnum
# e=65537
# c=[c1,c2,c3,c4...]
# n=[n1,n2,n3,n4...]
for i in range(len(n)):
	for j in range(i+1,len(n)):
		d=gmpy2.gcd(n[i],n[j])
		if d!=1:
			p=d
			q=n[i]//p
			d=gmpy2.invert(e,(p-1)*(q-1))
			m=pow(c[i],d,n[i])
			print(libnum.n2s(int(m)))
```
#### 低解密指数攻击
