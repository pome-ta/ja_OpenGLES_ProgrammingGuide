# Checklist for Building OpenGL ES Apps for iOS

OpenGL ES仕様は、GPUハードウェアを使用してグラフィックスをレンダリングするためのプラットフォームに依存しないAPIを定義しています。OpenGL ESを実装するプラットフォームは、OpenGL ESコマンドを実行するためのレンダリングコンテキスト、レンダリング結果を保持するためのフレームバッファー、および表示用のフレームバッファーの内容を提示する1つ以上のレンダリング先を提供します。iOSでは、EAGLContextクラスはレンダリングコンテキストを実装します。iOSは、OpenGL ESフレームバッファオブジェクトという1種類のフレームバッファのみを提供し、GLKViewクラスとCAEAGLLayerクラスはレンダリング先を実装します。


iOSでOpenGLESアプリを構築するには、いくつかの考慮事項が必要です。その中には、OpenGL ESプログラミングに一般的なものもあれば、iOSに固有のものもあります。 開始するには、このチェックリストと以下の詳細なセクションに従ってください。

1. アプリに適した機能セットを備えたOpenGLESのバージョンを特定し、OpenGLESコンテキストを作成します。
1. 実行時に、デバイスが使用するOpenGLES機能をサポートしていることを確認します。
1. OpenGLESコンテンツをレンダリングする場所を選択します。
1. アプリがiOSで正しく実行されることを確認してください。
1. レンダリングエンジンを実装します。
1. XcodeとInstrumentsを使用して、OpenGL ESアプリをデバッグし、最適なパフォーマンスになるように調整します。



## Choosing Which OpenGL ES Versions to Support

> サポートするOpenGLESバージョンの選択


アプリがOpenGLES 3.0、OpenGL ES 2.0、OpenGL ES 1.1、または複数のバージョンをサポートする必要があるかどうかを決定します。


- OpenGL ES3.0はiOS7の新機能です。これにより、以前はデスクトップクラスのハードウェアおよびゲームコンソールでのみ可能であった、より高いパフォーマンス、汎用GPUコンピューティング技術、およびより複雑な視覚効果を可能にする多くの新機能が追加されます。
- OpenGL ES 2.0は、iOSデバイスのベースラインプロファイルであり、プログラム可能なシェーダーに基づく構成可能なグラフィックスパイプラインを備えています。
- OpenGL ES 1.1は、基本的な固定機能のグラフィックパイプラインのみを提供し、主に下位互換性のためにiOSで使用できます。


アプリに最も関連する機能とデバイスをサポートする1​​つまたは複数のバージョンのOpenGLESをターゲットにする必要があります。  iOSデバイスのOpenGLES機能の詳細については、iOSデバイス互換性リファレンスを参照してください。


サポートする予定のOpenGLESのバージョンのコンテキストを作成するには、「OpenGLESコンテキストの構成」を参照してください。  OpenGL ESバージョンの選択が、アプリで使用する可能性のあるレンダリングアルゴリズムとどのように関連しているかについては、OpenGLESバージョンとレンダラーアーキテクチャを参照してください。



## Verifying OpenGL ES Capabilities

> OpenGLES機能の検証


iOSデバイス互換性リファレンスには、iOSデバイスの出荷で利用できる機能と拡張機能がまとめられています。 ただし、アプリをできるだけ多くのデバイスとiOSバージョンで実行できるようにするには、アプリは常にOpenGLES実装に実行時の機能を問い合わせる必要があります。



最大テクスチャサイズや頂点属性の最大数などの実装固有の制限を決定するには、データ型に適切なglGet関数を使用して、対応するトークン（gl.hヘッダーにある`MAX_TEXTURE_SIZE`や`MAX_VERTEX_ATTRIBSなど`）の値を検索します。



OpenGL ES 3.0拡張機能を確認するには、次のコード例のように`glGetIntegerv`関数と`glGetStringi`関数を使用します。


``` .cpp
BOOL CheckForExtension(NSString *searchName)
{
    // Create a set containing all extension names.
    // すべての拡張子名を含むセットを作成します。
    // (For better performance, create the set only once and cache it for future use.)
    // （パフォーマンスを向上させるには、セットを1回だけ作成し、将来使用するためにキャッシュします。）
    int max = 0;
    glGetIntegerv(GL_NUM_EXTENSIONS, &max);
    NSMutableSet *extensions = [NSMutableSet set];
    for (int i = 0; i < max; i++) {
        [extensions addObject: @( (char *)glGetStringi(GL_EXTENSIONS, i) )];
    }
    return [extensions containsObject: searchName];
}

```


OpenGL ES 1.1および2.0拡張機能を確認するには、`glGetString(GL_EXTENSIONS)`を呼び出して、すべての拡張機能名のスペース区切りのリストを取得します。



## Choosing a Rendering Destination

> レンダリング先の選択


iOSでは、フレームバッファオブジェクトは描画コマンドの結果を保存します。（iOSはウィンドウシステムが提供するフレームバッファを実装していません。）フレームバッファオブジェクトのコンテンツは複数の方法で使用できます。


- GLKitフレームワークは、OpenGL ESコンテンツを描画し、独自のフレームバッファーオブジェクトを管理するビューと、OpenGLESコンテンツのアニメーション化をサポートするビューコントローラーを提供します。これらのクラスを使用して、フルスクリーンビューを作成したり、OpenGLESコンテンツをUIKitビュー階層に適合させたりします。 これらのクラスについては、OpenGLESおよびGLKitを使用した描画を参照してください。
- CAEAGLLayerクラスは、CoreAnimationレイヤーコンポジションの一部としてOpenGLESコンテンツを描画する方法を提供します。 このクラスを使用するときは、独自のフレームバッファオブジェクトを作成する必要があります。
- 他のOpenGLES実装と同様に、オフスクリーングラフィックス処理またはグラフィックスパイプラインの他の場所で使用するテクスチャへのレンダリングにフレームバッファを使用することもできます。  OpenGL ES 3.0では、複数のレンダリングターゲットを利用するレンダリングアルゴリズムでオフスクリーンバッファを使用できます。


オフスクリーンバッファ、テクスチャ、またはCore Animationレイヤーへのレンダリングについては、「他のレンダリング先への描画」を参照してください。



## Integrating with iOS

> iOSとの統合


iOSアプリはデフォルトでマルチタスクをサポートしていますが、OpenGLESアプリでこの機能を正しく処理するには追加の考慮が必要です。OpenGL ESを不適切に使用すると、バックグラウンドでアプリがシステムによって強制終了される可能性があります。


多くのiOSデバイスには高解像度ディスプレイが含まれているため、アプリは複数のディスプレイサイズと解像度をサポートする必要があります。


これらおよびその他のiOS機能のサポートについては、マルチタスク、高解像度、およびその他のiOS機能をお読みください。



## Implementing a Rendering Engine

> レンダリングエンジンの実装


OpenGL ES描画コードを設計するための多くの可能な戦略がありますが、その詳細はこのドキュメントの範囲を超えています。レンダリングエンジン設計の多くの側面は、OpenGLおよびOpenGLESのすべての実装に共通しています。


iOSデバイスにとって重要な設計上の考慮事項については、OpenGLESの設計ガイドラインと同時実行性およびOpenGLESを参照してください。


## Debugging and Profiling

> デバッグとプロファイリング


XcodeとInstrumentsは、レンダリングの問題を追跡し、アプリのOpenGLESパフォーマンスを分析するための多数のツールを提供します。


OpenGL ESアプリの問題の解決とパフォーマンスの向上について詳しくは、OpenGLESアプリのチューニングをご覧ください。


