# Solidity
Solidityのコードは*コントラクト*内にカプセル化されている。コントラクトはイーサリアムアプリケーションの基本ブロックのことだ。変数やファンクションはすべてコントラクトに属している。これがプロジェクトの出発点になる。

### *Version pragma(バージョンのプラグマ)*
Solidityのソースコードは全て”version pragma”で始まらなければならない。コードが使用するSolidityのコンパイラのバージョンを宣言するものだ。これは、将来コンパイラのバージョンが原因でコードが壊れることを防ぐために必要なのだ。
実際にはpragma solidity ^0.4.19; という風に宣言する（この解説を書いている時点での最新のsolidityのバージョンは0.4.19)。
これをまとめれば、コントラクトを開始するための骨組みが出来上がる

```
pragma solidity ^0.4.19;　//version pragma

//コントラクト
contract ZombieFactory {

}
```


## *状態変数と整数*
*状態変数* はコントラクト内に永遠に保管され続けるものだ。要するにイーサリアムブロックチェーン上に記載されるということだ。まぁDB（データベース）に書き込むようなものだと思って良い。
### *符号なし整数:**uint*
uintというのは、符号なし整数のデータ型で、こいつは 負数ではないということを示しているのだ。この他にintという符号付整数もあるから覚えておくようにな。
/注: Solidityでは、//uint//は256ビットの符号なし整数である//uint256//のエイリアスです。//uint8//、//uint16//、//uint32//など、少ないビット数でuintを宣言することもできます。しかし、一般的には、後のレッスンで説明するような特定の場合を除いて、単に//uint//を使います。/
```
pragma solidity ^0.4.19;

contract ZombieFactory {

    uint dnaDigits = 16;

}

```


## *構造体*
複雑なデータ型が必要になる場合がある。Solidityはそのために*構造体*を用意している
構造体を使えば、複数のプロパティを持つ複雑なデータ型を作成することができる。
/新しいデータ型である//string//を導入しました。Stringは任意の長さのUTF-8データに使用されます。例：//string greeting = “Hello world!”/
```
pragma solidity ^0.4.19;

contract ZombieFactory {

    uint dnaDigits = 16;
    uint dnaModulus = 10 ** dnaDigits;

    //structで構造体を定義
    struct Zombie {
        string name;
        uint dna;
    }

}
```


## *配列*
何かのコレクションを作りたければ、*_配列_*を利用するのだ。Solidityには2種類の配列が用意されている：*固定長*配列と*可変長*配列だ
/// 2要素の固定長の配列の場合：/
uint[2] fixedArray;
/// 別の固定長配列の例。5つの文字列を格納できる：/
string[5] stringArray;
/// 可変長配列 - 決まったサイズはないので、格納し続けることができるぞ：/
uint[] dynamicArray;

構造体の配列も作れるぞ。Personという構造体を作成している場合
Person/[]/ people; // このように可変長配列で書ける。さらに追加し続けることもできるぞ。
状態変数は永久にブロックチェーン上に格納されると話したことを覚えているか？こういう構造体の可変長配列は、データベースのように使えるから、コントラクトの構造データを格納する時に便利だ。
## パブリックの配列
配列をpublicで宣言すれば、Solidityが自動的にgetterメソッドを作成するぞ。コードの書き方はこうだ：
Person/[]/ public people;
他のコントラクトもこの配列を読める（但し、書き込めない）ぞ。こういう性質を持っているから、コントラクトの公開データを格納するときに便利に使えるパターンとして覚えておくように。
```
pragma solidity ^0.4.19;

contract ZombieFactory {

    uint dnaDigits = 16;
    uint dnaModulus = 10 ** dnaDigits;

    struct Zombie {
        string name;
        uint dna;
    }
    //publicなZombie構造体の配列を定義(publicなため、他のコントラクトもこのデータを読める)
    Zombie[] public zombies;

}
```


## 関数の宣言
関数の宣言はsolidityでは次のようになる：
function eatHamburgers(string _name, uint _amount) {

}
これはstring と uintという2つのパラメーターを持つ、eatHamburgersというファンクションだ。関数の中身は今は空にしてある。
/注：グローバル変数と区別をつけるために、関数パラメーター変数名はアンダースコア(//_//)をつけるのが通例（必須ではありません）です。このチュートリアルでは通例に従います。/
この関数は次のように呼び出すことが可能だ：
eatHamburgers(“vitalik”, 100);
```
pragma solidity ^0.4.19;

contract ZombieFactory {

    uint dnaDigits = 16;
    uint dnaModulus = 10 ** dnaDigits;

    struct Zombie {
        string name;
        uint dna;
    }

    Zombie[] public zombies;

    // 関数宣言
    function createZombie(string _name, uint _dna) {

    }

}
```


## *構造体と配列の操作*
struct Person {
  uint age;
  string name;
}

Person[] public people;

people### 配列に格納する方法を教えるぞ。
/// 新しいPersonを作る：/
Person satoshi = Person(172, “Satoshi”);

/// それを配列に格納する：/
people.push(satoshi);
### 全部まとめて1行で書けば、さらにスッキリできるぞ：
people.push(Person(16, “Vitalik”));
array.push()### は配列の
### 最後
### に何かを追加するので、要素は追加した順番になります。次の例を参照してください：
uint[] numbers;
numbers.push(5);
numbers.push(10);
numbers.push(15);
/// 数字は [5, 10, 15]/

```
pragma solidity ^0.4.19;

contract ZombieFactory {

    uint dnaDigits = 16;
    uint dnaModulus = 10 ** dnaDigits;

    struct Zombie {
        string name;
        uint dna;
    }

    Zombie[] public zombies;

    function createZombie(string _name, uint _dna) {
        // 新しい構造体を作り、配列に格納する
        Zombie zombie1 = Zombie(_name, _dna);
        zombies.push(zombie1);
        // 上の作業を1行で行う
        zombies.push(Zombie(_name, _dna));
    }

}
```


## *Private / Public 関数*
Solidityでは、関数はデフォルトでpublicになっている。要するに誰でも（別のコントラクトからでも）君のコントラクトの関数を呼び出して、実行できるということだ。
これは当然愉快なことではないし、攻撃に対してコントラクトが脆弱になることになる。だから、自分が使う関数はデフォルトでprivateにして、公開しても構わない関数だけをpublicに設定するのだと、心がけておくように。
では、private関数の宣言方法を教えるぞ：
uint[] numbers;

function _addToArray(uint _number) private {
  numbers.push(_number);
}
このように書くと、この関数はコントラクト内の他の関数からだけ呼び出して、numbers配列に格納できる。
見ればわかる通り、関数名の後に privateとつけるのだ。関数のパラメーターと同様に、private関数はアンダースコア(_)で始めるのが通例だから覚えておくように。
```
pragma solidity ^0.4.19;

contract ZombieFactory {

    uint dnaDigits = 16;
    uint dnaModulus = 10 ** dnaDigits;

    struct Zombie {
        string name;
        uint dna;
    }

    Zombie[] public zombies;
    
    // private関数(アンダースコアで始める)
    function _createZombie(string _name, uint _dna) private {
        zombies.push(Zombie(_name, _dna));
    }

}
```

## 戻り値
関数から値を返すときは、次のように宣言するのだ：
string greeting = “What’s up dog”;

function sayHello() public returns (string) {
  return greeting;
}
Solidityでは関数の宣言に戻り値の型を含むから覚えておくように（ここでは stringだ）。
## 関数の修飾子
上の関数はSolidity上では何も変更されないぞ。例えば値を変更したり、何かを書き込むこともない。
このケースではview関数を宣言できる。これはつまりデータの読み取り専用で編集できないということだ：
function sayHello() public view returns (string) {
Solidityにはpure関数がある。これを使うとアプリ内のデータにすらアクセスできない。次のコードを考えてみよう：
function _multiply(uint a, uint b) private pure returns (uint) {
  return a * b;
}
この関数はアプリから読み込むことすらできない。つまり戻り値が関数のパラメーターのみに依存することになる。この場合pure関数として宣言することができる。
/注：どんなときに関数をpure/viewと修飾するか覚えるのは難しいかもしれません。幸い、Solidityのコンパイラは優秀なので、どちらの修飾子を使うべきか警告してくれます。/
```
pragma solidity ^0.4.19;

contract ZombieFactory {

    uint dnaDigits = 16;
    uint dnaModulus = 10 ** dnaDigits;

    struct Zombie {
        string name;
        uint dna;
    }

    Zombie[] public zombies;

    function _createZombie(string _name, uint _dna) private {
        zombies.push(Zombie(_name, _dna));
    } 

    function _generateRandomDna(string _str) private view returns (uint) {
        
    }

}
```


*Keccak256と型キャスト*
イーサリアムにはSHA3のバージョンの一つであるkeccak256が組み込まれている。ハッシュ関数は基本的には、文字列をランダムな256ビットの16進数にマッピングする機能だ。文字列をほんの少しでも変更すれば、ハッシュは大きく変わるから気をつけるようにな。
イーサリアムのいろいろな場面で使用できるが、ここでは擬似乱数生成に使用していくぞ。
例：
///6e91ec6b618bb462a4a6ee5aa2cb0e9cf30f7a052bb467b0ba58b8748c00d2e5/
keccak256(“aaaab”);
///b1f078126895a1424524de5321b339ab00408010b7cf0e6ed451514981e58aa9/
keccak256(“aaaac”);
見てわかると思うが、入力する文字が１文字違うだけで、戻り値が全く別物になることがわかったかな。
/注：ブロックチェーンでの//安全な//乱数の生成は非常に難しい課題です。ここで紹介する方法は安全なものではありませんが、ゾンビDNAの作成のチュートリアルではセキュリティを考慮する必要はないので、この方法で十分です。/
## 型キャスト
場合によっては、データ型を変更する必要があるときがある。下の例で考えてみるぞ：
uint8 a = 5;
uint b = 6;
/// a * b はuint8ではなくuintで返すから、エラーになる：/
uint8 c = a * b; 
/// 正しく動作させるために、bをuint8に型キャストさせる：/
uint8 c = a * uint8(b); 
この例ではa * bはuintを返すが、uint8で格納しようとしているから、問題が発生することになる。uint8にキャストすることで、正常に動作する上にコンパイラもエラーを吐き出すことがなくなる。
```
pragma solidity ^0.4.19;

contract ZombieFactory {

    uint dnaDigits = 16;
    uint dnaModulus = 10 ** dnaDigits;

    struct Zombie {
        string name;
        uint dna;
    }

    Zombie[] public zombies;

    function _createZombie(string _name, uint _dna) private {
        zombies.push(Zombie(_name, _dna));
    } 

    function _generateRandomDna(string _str) private view returns (uint) {
        uint rand = uint(keccak256(_str));
        return rand % dnaModulus;
    }

}


```


## *イベント*
### Events
### は、ブロックチェーンで何かが生じたときに、コントラクトがアプリのフロントエンドに伝えることができるものだ。しかも特定のイベントを’listening’状態にして、何かあった時にアクションを起こすこともできるのだ。
### 例：
/// イベントの宣言/
event IntegersAdded(uint x, uint y, uint result);

function add(uint _x, uint _y) public {
  uint result = _x + _y;
  /// 関数が呼ばれたことをアプリに伝えるためにイベントを発生させる：/
  IntegersAdded(_x, _y, result);
  return result;
}
### アプリのフロントエンドをリッスン（接続待ち）状態にできる。JavaScriptで実装すると次のように書けるのだ：
YourContract.IntegersAdded(function(error, result) {
  /// 結果について何らかの処理をする/
})
```
pragma solidity ^0.4.19;

contract ZombieFactory {

    // イベントの宣言
    event NewZombie(uint zombieId, string name, uint dna);

    uint dnaDigits = 16;
    uint dnaModulus = 10 ** dnaDigits;

    struct Zombie {
        string name;
        uint dna;
    }

    Zombie[] public zombies;

    function _createZombie(string _name, uint _dna) private {
        uint id = zombies.push(Zombie(_name, _dna)) - 1;
        NewZombie(id, _name, _dna);
    }

    function _generateRandomDna(string _str) private view returns (uint) {
        uint rand = uint(keccak256(_str));
        return rand % dnaModulus;
    }

    function createRandomZombie(string _name) public {
        uint randDna = _generateRandomDna(_name);
        // イベントを発生させる
        _createZombie(_name, randDna);
    }

}

```
