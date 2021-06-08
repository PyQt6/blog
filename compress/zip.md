
# 压缩 (zip已经是压缩的代名词了)

注意，压缩格式与压缩算法是不一样的，压缩后小不小主要取决于压缩算法而不是格式。

压缩率 = 压缩后大小 / 压缩前大小 * 100%     
(压缩率 <= 1)

## 压缩格式(打包格式)

所谓的"压缩格式"实质就是打包格式，只是把几个文件打包成一个文件，并没有压缩。tar是Linux下最典型的打包格式。

打包格式 != 文件扩展名

真·打包格式:
1. zip (默认使用Deflate)
2. 7z (默认使用LZMA)
3. tar (默认不压缩)
4. wim (默认不压缩)
5. rar

其他文件扩展名:
- xz (xzip)
- gz (gzip)
- bz2 (bzip2)
- liz (lizard)
- zst (zstd)
- lz4
- lz5


关于rar
> rar是一种专利格式，只有rar自己的软件(比如winrar)才能压缩，但其他软件也可以解压。不推荐使用rar


## 压缩算法

1. Deflate (zip,7z,gz使用)
2. Deflate64 (zip,7z使用)
3. LZMA (zip,7z使用)
4. LZMA2 (7z,xz使用)
5. LZ4 (7z,lz4使用)
6. LZ5 (7z,lz5使用)
7. PPMd (zip,7z使用)
8. BZip2 (zip,7z,bz2使用)
9. Lizard系列 (7z,liz使用)
10. ZStandard (zip,7z,zst使用)


## zip

zip是一种稀疏压缩格式，支持单独解压其中一个文件，支持对每个文件设置不同的密码。

zip支持的加密算法有
- ZipCrypto (容易被破解，易被已知明文攻击)
- AES-128
- AES-192
- AES-256

CTF比赛中可能会涉及zip伪加密、zip已知明文攻击

## 7z

7z是一种固实压缩格式，不支持单文件单独解压，不支持对每个文件设置不同的密码(因为是先压成一坨再加密)。

7z支持的加密算法只有
- AES-256

## 测试结果

一般情况下，同一个文件压缩后的大小:

7z(LZMA)≈xz(LZMA2)<zst(ZStandard)<zip(Deflate[64])≈rar<bz2<gz

同一个文件的压缩时间:

zip<rar<7z

因为7z是固实压缩

注:
> Windows下zip压缩很快，gz很慢；Linux下刚好相反。

