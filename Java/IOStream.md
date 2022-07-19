# IOStream
ストリームとは
→入力ソースや出力ソースに対する論理的なハンドリング
例：Udemyは出力ストリームを使ってデータをWebブラウザに転送する
　　ブラウザのビデオプレーヤーはそのデータを読み込むためにインプットストリームを使う

javaは4つのストリームをサポートしている
* ByteStreams
* CharacterStreams
* BufferStreams
* ObjectStreams
*これらのストリームはすべてjava.ioパッケージに属している*
ByteStreams
→1バイトずつ読み書きを行う（バイナリ情報を読み込む）
CharacterStreams
→unicodeを使用する（文字情報を読み込む）
BufferStreams
→上二つのラッパー（より多くのデータを読み取る）
ObjectStreams
→ファイルシステムやネットワークへのオブジェクトの読み書きを可能にする（オブジェクトの読み取りと書き込みに使用される）

```
package outputstreams;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;

/*
 * FileInputStreamとFileOutputStreamを使用して
 * 画像ファイルのコピーを行う
 */
public class FileOutputStreamDemo {

	public static void main(String[] args) {
		
		FileInputStream fis = null;
		FileOutputStream fos = null;
		
		try {
			//コピー元のファイルパスを指定（データのインプット）
			fis = new FileInputStream("/Users/odataiyou/test/IMG_0916.PNG");
			//コピー先のファイルパスを指定（データのアウトプット）
			fos = new FileOutputStream("/Users/odataiyou/test/new.PNG");
			
			int data;
			//コピー元ファイルのデータをコピー先ファイルに書き込む
			while((data=fis.read()) != -1) {
				fos.write(data);
			}
			System.out.println("File copied");
			
			//FileInputStreamの例外処理
		} catch (FileNotFoundException e) {
			e.printStackTrace();
			//readの例外処理
		} catch (IOException e) {
			e.printStackTrace();
		} finally {
			//開いたファイルを閉じる
			try {
				fis.close();
				fos.close();
			} catch (IOException e) {
				e.printStackTrace();
			}
		}
		
	}

}

```
