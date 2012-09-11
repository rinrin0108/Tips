## warファイルとは
- war=Web Application Resources
- Webアプリケーションに関するリソースを圧縮したもの
    - zip形式
- .warファイルはアプリケーションディレクトリ配下に配置することで、コンテナ再起動時に自動的にデプロイされ、クライアントからアクセス可能な状態になる
    - Tomcatの場合、アプリケーションディレクトリは「Tomcat○○○\webapps」（Windows版）、「jakarta-tomcat-○○○/webapps」（Linux版）
    - 例外
        - Tomcatでは、Server.xmlのHost要素においてunpackWARs属性が“false”に設定されている場合、warファイルの自動展開は行われない
        - また、Server.xmlに該当するコンテキスト情報（Context要素）が書き込まれている場合にもwarファイルが自動展開されないことがある
            - その場合には該当のContext要素を削除してから、コンテナの再起動を行う
- J2EEの標準的なファイルで、ユーザー側もアプリケーションごとにインストール方法が異なるなどで悩む必要がない

## warファイルを作成する
- jarコマンドを利用する方法（%CATALINA_HOME%/webapps/hogehogeフォルダ内のファイル群をwarファイルに格納したい場合）
    1. cd %CATALINA_HOME%/webapps/hogehoge
    2. jar cvf hogehoge.war *

- Eclipseを利用する方法
    1. 「Tomcat PlugIn」を準備する
        - Sysdeo Eclipse Tomcat Launcher pluginを利用
    2. 「.war」ファイルの出力先を指定する
        1. パッケージエクスプローラからプロジェクト名を右クリック、表示されたコンテキストメニューから「プロパティ（R）」を選択
        2. 「（アプリケーション名）のプロパティ」が開くので、左のツリーから「Tomcat」を選択したうえで、［全般］タブの「エクスポートするWARファイル」を指定
            - この際に「.javaファイルを含める」かどうかを指定することができる。エンドユーザーにソースコードを公開したくない場合にはボックスにチェックを立てることで、「.war」ファイル作成時に「.java」ファイルを除外する
    2. 「.war」ファイルを作成する（2回目以降の.warファイル作成においては、この作業だけを行えばOK）
        1. パッケージ・エクスプローラからプロジェクト名を右クリック、表示されたコンテキストメニューから［Tomcatプロジェクト］→［プロジェクト設定に従いWARファイルを作成する］を選択
        2. ダイアログが表示され、設定したパス名で.warファイルができていれば成功

## warファイルの標準構成
|>|>|>|CENTER:*.war|
|-|-|-|-|
|css/|>|>|スタイルシート|
|images/|>|>|イメージファイル|
|META-INF/|>|>|メタデータファイル|
|WEB-INF/|classes/|>|クラスファイル|
|~|~|resources/|リソースファイル|
|~|lib/|>|JARファイル|
|~|compass/|>|Compass設定ファイル|
|~|ibatis/|>|iBATIS設定ファイル|
|~|jaxrpc/|>|JAX-RPCマッピング|
|~|quartz/|>|Quartz設定ファイル|
|~|seasar/|>|Seasar設定ファイル|
|~|spring/|>|Spring設定ファイル|
|~|struts/|>|Struts設定ファイル|
|~|tld/|>|タグライブラリ設定ファイル|
|~|velocity/|>|Velocity設定ファイル|
|~|view/|>|表示系テンプレート|
|~|wsdl/|>|WSDLファイル|
|~|xml/|>|XMLテンプレート|
|~|jboss-web.xml|>|JBoss設定ファイル|
|~|web.xml|>|Webアプリケーション設定ファイル|
|~|webservices.xml|>|Webサービス設定ファイル|
|index.html|>|>|サイトインデックス|


## webアプリケーションの作成
- Tomcatプロジェクトの作成
    1. File-New-Projectを選択、Java-TomcatProjectを選択してNext
    2. プロジェクト名を入力
    3. Finishを選択
- JSPの作成
    1. プロジェクトのルートにファイルを新規作成（xxxxx.jsp）
    2. jspファイルを編集
- Tomcat起動
    1. toolbar-Tomcat開始ボタン
    2. ブラウザからアクセス
    3. http://localhost:8080/[プロジェクト名]/[JSPのファイル名].jsp
- サーブレットの作成
    1. WEB-INF/srcの下にパッケージを追加
        - 例：「nri.ossc.test」
    2. 作成したパッケージの下にjavaファイルを作成
    3. Name欄にクラス名を入力
    4. Superclassには「javax.servlet.http.HttpServlet」を指定する
    5. Finishを選択
- javaファイルを編集
    1. 警告が表示されているので、コンテキストメニューから@SuppressWarningを選択して警告を消す
    2. クラス定義内にカレットがある状態でCtrl+スペースキーを押す
    3. Eclipseの入力支援機能によってメニューが表示されるので、doGetを選択
    4. doGetメソッドを以下の内容に書き換える
        - PrintWriter w = resp.getWriter();
        - w.println("Hello, Servlet!");
- web.xmlの作成
    1. WEB-INF/の下にweb.xmlファイルを作成する
    2. web.xmlを以下の通り編集する


    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE web-app
         PUBLIC "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
         "http://java.sun.com/dtd/web-app_2_3.dtd">
    <web-app>
        <servlet>
            <servlet-name>ServletTest1</servlet-name>
            <servlet-class>nri.ossc.test.ServletTest1</servlet-class>
        </servlet>
        <servlet-mapping>
             <servlet-name>ServletTest1</servlet-name>
             <url-pattern>/servlettest1</url-pattern>
       </servlet-mapping>
    </web-app>


----
参考資料：
- http://www.atmarkit.co.jp/fjava/javatips/043eclipse016.html
- http://www.atmarkit.co.jp/fjava/javatips/040jspservlet029.html
- http://webframe.sourceforge.jp/wiki/index.php?WAR%A5%D1%A5%C3%A5%B1%A1%BC%A5%B8%C9%B8%BD%E0%A5%C7%A5%A3%A5%EC%A5%AF%A5%C8%A5%EA%B9%BD%C0%AE
- http://java.keicode.com/lang/web-tomcat.php
