
# 代数学、近世代数 algebra


## 二次剩余

### 密码学常见问题

n=p*q, p,q素

$x^2=a^2 \mod n$

有4个根$z_1,z_2,z_3,z_4$

证:
因为 $x^2=a^2 \mod p$ 有两根 $x_1=a,x_2=p-a$    
$x^2=a^2 \mod q$ 有两根 $x_1'=a,x_2'=q-a$

所以得到4个CRT(中国剩余定理)方程

$$
\left\{
\begin{aligned}
    z_1&=a \mod p \\
    z_1&=a \mod q
\end{aligned}
\right.

\newline[5pt]%\newline等价于\\

\left\{
\begin{aligned}
    z_2&=a \mod p \\
    z_2&=q-a \mod q
\end{aligned}
\right.

\\[5pt]

\left\{
\begin{aligned}
    z_3&=p-a \mod p \\
    z_3&=a \mod q
\end{aligned}
\right.

\\[5pt]

\left\{
\begin{aligned}
    z_4&=p-a \mod p \\
    z_4&=q-a \mod q
\end{aligned}
\right.

$$

最终解为
$x=z_1,z_2,z_3,z_4 \mod n$

它们满足(性质):   
$z_1+z_4=n$     
$z_2+z_3=n$     
$\gcd(z_1+z_2,n)=\gcd(z_3+z_4,n)=q$     
$\gcd(z_1+z_3,n)=\gcd(z_2+z_4,n)=p$






