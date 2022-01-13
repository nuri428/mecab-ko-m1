# mecab-ko-m1 소개
해당 프로젝트는 mecab-ko에서 애플에서 발표한 M1 아키텍춰에 맞게 빌드 옵션을 수정한 프로젝트입니다.

기본적으로 mecab-ko와 같으나 빌드 옵션많이 추가 되어 m1을 사용하시는 분들께서 로제타 바이너리를 통하지 않고 arm64 환경에서 바로 사용할수 있습니다. 

설치 옵션은 하단에 있는 설치 방법과 동일합니다. 

## m1에서 설치시 확인점 
해당 프로젝트를 clone 한 후 아래와 같은 명령을 입력하여 나온 결과를 확인합니다. 

    :::text
    $ tar zxfv mecab-ko-XX.tar.gz
    $ cd mecab-ko-XX
    $ config.guess
    aarch64-apple-darwin21.2.0
    

기존의 mecab-ko에서는 config.guess 실행 결과가 arm-apple-darwin*으로 나오나 설정 파일을 수정하여 aarch64가 나오게 수정 하였습니다. 

설치가 끝난후 dylib를 확인 하면 다음과 같은 결과를 확인 할 수 있습니다.

    :::text
    $ file /usr/local/lib/libmecab.2.dylib
    /usr/local/lib/libmecab.2.dylib: Mach-O 64-bit dynamically linked shared library arm64

위와 같이 dylib 파일이 arm64 cpu 타입의 동적 라이브러리로 작성된것을 확인 할 수 있습니다. 

## python-mecab 설치 팁 
mecab-python을 설치시 pip를 통하지 않고 mecab-python을 git에서 클론 후, setuptools를 버젼업 시키고 설치를 추천 드립니다. 

    :::text
    $ git clone https://bitbucket.org/eunjeon/mecab-python-0.996.git
    $ pip install -U  setuptools
    $ cd mecab-python-0.996/
    $ python setup.py build
    $ python setup.py install 


# mecab-ko 소개

[mecab-ko](https://bitbucket.org/eunjeon/mecab-ko)는 [은전한닢 프로젝트](http://eunjeon.blogspot.kr/)에서 사용하기 위한 [MeCab](http://mecab.googlecode.com/svn/trunk/mecab/doc/index.html)의 fork 프로젝트 입니다.

최소한의 변경으로 한국어의 특성에 맞는 기능을 추가하는 것이 목표입니다.

# mecab-ko에서 추가된 기능.

## 공백 문자(white space)를 포함하는 특정 품사 비용 늘림

띄어쓰기를 하지 않는 일본어와 달리 띄어쓰기를 하는 한국어 특성에 맞게 특정 품사가 띄어쓰기 되어있는 경우 해당 품사의 비용을 늘리는 기능 (사전 설정(dicrc)에 설정 값을 지정)

__mecab을 사용하여 분석__

    :::text
    화학 이외의 것
    화학    NN,T,화학,*,*,*,*
    이      JKS,F,이,*,*,*,*
    외      NN,F,외,*,*,*,*
    의      JKG,F,의,*,*,*,*
    것      NNB,T,것,*,*,*,*
    EOS

__mecab-ko를 사용하여 분석__

    :::text
    화학 이외의 것
    화학    NN,T,화학,*,*,*,*
    이외    NN,F,이외,*,*,*,*
    의      JKG,F,의,*,*,*,*
    것      NNB,T,것,*,*,*,*
    EOS

### 설정 방법

MeCab의 사전 설정(dicrc)에서 다음과 같이 설정합니다.

    :::text
    # 좌측에 공백을 포함하는 품사의 연접 비용을 늘리기 위한 설정입니다.
    # mecab-ko에서만 사용되는 설정입니다. 다음과 같은 형식을 가집니다.
    # <posid 1>,<posid 1 penalty cost>,<posid 2>,<posid 2 penalty cost> ...
    # 
    # 예) 120,6000 => posid가 120인 품사(조사)의 좌측에 공백을 포함할 경우
    # 연접 비용을 6000만큼 늘림
    left-space-penalty-factor = 120,6000,184,6000,100,500

# mecab-ko의 설치와 사용법

## mecab-ko 설치

  [mecab-ko 다운로드 페이지](https://bitbucket.org/eunjeon/mecab-ko/downloads)에서 최신 버전의 소스를 다운 받고 설치합니다. tar.gz 압축을 해제하고 일반적인 자유 소프트웨어와 같은 순서로 설치할 수 있습니다.

    :::text
    $ tar zxfv mecab-ko-XX.tar.gz
    $ cd mecab-ko-XX
    $ ./configure 
    $ make
    $ make check
    $ su
    # make install

설치 방법은 MeCab와 동일하므로, 자세한 내용은 [MeCab 홈페이지](http://mecab.googlecode.com/svn/trunk/mecab/doc/index.html)를 참조하시기 바랍니다.

### 참고

  * 오래된 버전의 리눅스 환경에서 컴파일이 안되는 경우, 다음의 글을 참조하시기 바랍니다. [Cent OS 5.9에서 MeCab 및 mecab-ko-dic 설치하기](http://eunjeon.blogspot.kr/2013/02/cent-os-59-mecab-mecab-ko-dic.html)

## 한국어 사전(mecab-ko-dic)의 설치와 사용

  [mecab-ko-dic](https://bitbucket.org/eunjeon/mecab-ko-dic)의 설명을 참조하시기 바랍니다.

# 라이센스

mecab-ko의 라이센스는 MeCab의 라이센스를 그대로 따릅니다.

> MeCab 는 무료 소프트웨어입니다. GPL (the GNU General Public License), LGPL (Lesser GNU General Public License) 또는 BSD 라이선스에 따라 소프트웨어를 사용, 재배포할 수 있습니다. 자세한 내용은 COPYING, GPL, LGPL, BSD 각 파일을 참조하십시오.
