# Configuring OpenGL ES Contexts

> OpenGLESコンテキストの構成


OpenGL ESのすべての実装は、OpenGLES仕様で必要とされる状態を管理するためのレンダリングコンテキストを作成する方法を提供します。この状態をコンテキストに配置することで、複数のアプリが他のアプリの状態に干渉することなく、グラフィックハードウェアを簡単に共有できます。


この章では、iOSでコンテキストを作成および構成する方法について詳しく説明します。
 
 
 
## EAGL Is the iOS Implementation of an OpenGL ES Rendering Context

> EAGLは、OpenGLESレンダリングコンテキストのiOS実装です


アプリがOpenGLES関数を呼び出す前に、EAGLContextオブジェクトを初期化する必要があります。  EAGLContextクラスは、OpenGLESコンテンツをCoreAnimationと統合するために使用されるメソッドも提供します。



## The Current Context Is the Target for OpenGL ES Function Calls

> 現在のコンテキストがOpenGLES関数呼び出しのターゲットです



iOSアプリのすべてのスレッドには、現在のコンテキストがあります。OpenGL ES関数を呼び出すと、これは呼び出しによって状態が変更されるコンテキストです。



スレッドの現在のコンテキストを設定するには、そのスレッドで実行するときにEAGLContextクラスメソッド`setCurrentContext:` を呼び出します。


``` .cpp
[EAGLContext setCurrentContext: myContext];

```
EAGLContextクラスメソッドcurrentContextを呼び出して、スレッドの現在のコンテキストを取得します。


注：アプリが同じスレッド上の2つ以上のコンテキストをアクティブに切り替える場合は、新しいコンテキストを現在のコンテキストとして設定する前に、glFlush関数を呼び出してください。これにより、以前に送信されたコマンドがタイムリーにグラフィックハードウェアに配信されます。



OpenGL ESは、現在のコンテキストに対応するEAGLContextオブジェクトへの強力な参照を保持します。（手動参照カウントを使用している場合、OpenGL ESはこのオブジェクトを保持します。）setCurrentContext：メソッドを呼び出して現在のコンテキストを変更すると、OpenGLESは以前のコンテキストを参照しなくなります。（手動参照カウントを使用している場合、OpenGL ESはEAGLContextオブジェクトを解放します。）現在のコンテキストではないときにEAGLContextオブジェクトの割り当てが解除されないようにするには、アプリはこれらのオブジェクトへの強力な参照を保持（または保持）する必要があります。


## Every Context Targets a Specific Version of OpenGL ES

> すべてのコンテキストは、OpenGLESの特定のバージョンを対象としています


EAGLContextオブジェクトは、OpenGLESの1つのバージョンのみをサポートします。たとえば、OpenGL ES 1.1用に記述されたコードは、OpenGL ES2.0または3.0コンテキストと互換性がありません。OpenGL ES2.0のコア機能を使用するコードはOpenGLES 3.0コンテキストと互換性があり、OpenGL ES 2.0拡張用に設計されたコードは、多くの場合、わずかな変更を加えてOpenGL ES3.0コンテキストで使用できます。多くの新しいOpenGLES 3.0機能と強化されたハードウェア機能には、OpenGL ES3.0コンテキストが必要です。


アプリは、EAGLContextオブジェクトを作成および初期化するときに、サポートするOpenGLESのバージョンを決定します。デバイスが要求されたバージョンのOpenGLESをサポートしていない場合、initWithAPI：メソッドはnilを返します。アプリは、使用する前に、コンテキストが正常に初期化されたことを確認するためにテストする必要があります。


アプリのレンダリングオプションとしてOpenGLESの複数のバージョンをサポートするには、最初に、ターゲットにする最新バージョンのレンダリングコンテキストの初期化を試みる必要があります。返されたオブジェクトがnilの場合は、代わりに古いバージョンのコンテキストを初期化します。 リスト2-1は、これを行う方法を示しています。


**リスト2-1:** 同じアプリで複数のバージョンのOpenGLESをサポートする


``` .cpp
EAGLContext* CreateBestEAGLContext()
{
   EAGLContext *context = [[EAGLContext alloc] initWithAPI:kEAGLRenderingAPIOpenGLES3];
   if (context == nil) {
      context = [[EAGLContext alloc] initWithAPI:kEAGLRenderingAPIOpenGLES2];
   }
   return context;
}

```


コンテキストのAPIプロパティは、コンテキストがサポートするOpenGLESのバージョンを示します。アプリはコンテキストのAPIプロパティをテストし、それを使用して正しいレンダリングパスを選択する必要があります。この動作を実装するための一般的なパターンは、レンダリングパスごとにクラスを作成することです。アプリはコンテキストをテストし、初期化時にレンダラーを1回作成します。


