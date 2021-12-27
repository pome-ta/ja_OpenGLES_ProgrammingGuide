# About OpenGL ES

> 重要：OpenGLESはiOS12で非推奨になりました。GPUで高性能コードを作成するには、代わりにMetalフレームワークを使用してください。 [Metal](https://developer.apple.com/metal/)を参照してください。


Open Graphics Library（OpenGL）は、2Dおよび3Dデータを視覚化するために使用されます。 これは、2Dおよび3Dデジタルコンテンツ作成、機械および建築設計、仮想プロトタイピング、飛行シミュレーション、ビデオゲームなどのアプリケーションをサポートする多目的オープンスタンダードグラフィックライブラリです。OpenGLを使用して、3Dグラフィックスパイプラインを構成し、それにデータを送信します。頂点は変換および照明され、プリミティブにアセンブルされ、ラスタライズされて2Dイメージが作成されます。OpenGLは、関数呼び出しを、基盤となるグラフィックハードウェアに送信できるグラフィックコマンドに変換するように設計されています。 この基盤となるハードウェアはグラフィックコマンドの処理専用であるため、OpenGLの描画は通常非常に高速です。


OpenGL for Embedded Systems（OpenGL ES）は、OpenGLの簡略化されたバージョンであり、冗長な機能を排除して、モバイルグラフィックスハードウェアでの学習と実装の両方が容易なライブラリを提供します。


[画像リンク](#)




## At a Glance

> 一覧


OpenGL ESを使用すると、アプリは基盤となるグラフィックプロセッサの能力を活用できます。iOSデバイスのGPUは、高度な2Dおよび3D描画を実行できるだけでなく、最終画像のすべてのピクセルに対して複雑なシェーディング計算を実行できます。 アプリの設計要件でGPUハードウェアへの可能な限り最も直接的で包括的なアクセスが必要な場合は、OpenGLESを使用する必要があります。OpenGL ESの一般的なクライアントには、3Dグラフィックスを表示するビデオゲームやシミュレーションが含まれます。


OpenGL ESは、低レベルのハードウェアに焦点を合わせたAPIです。最も強力で柔軟なグラフィックス処理ツールを提供しますが、学習曲線が急で、アプリの全体的なデザインに大きな影響を与えます。より特殊な用途のために高性能グラフィックスを必要とするアプリのために、iOSはいくつかのより高いレベルのフレームワークを提供します。


- Sprite Kitフレームワークは、2Dゲームの作成に最適化されたハードウェアアクセラレーションアニメーションシステムを提供します。（[SpriteKit Programming Guide](https://developer.apple.com/library/archive/documentation/GraphicsAnimation/Conceptual/SpriteKit_PG/Introduction/Introduction.html#//apple_ref/doc/uid/TP40013043) を参照してください。）
- Core Imageフレームワークは、静止画像とビデオ画像のリアルタイムのフィルタリングと分析を提供します。（[Core Image Programming Guide](https://developer.apple.com/library/archive/documentation/GraphicsImaging/Conceptual/CoreImaging/ci_intro/ci_intro.html#//apple_ref/doc/uid/TP30001185)  を参照してください。）
- Core Animationは、すべてのiOSアプリにハードウェアアクセラレーションによるグラフィックレンダリングとアニメーションインフラストラクチャを提供するとともに、洗練されたユーザーインターフェイスアニメーションの実装を簡単にする単純な宣言型プログラミングモデルを提供します。  （[Core Animation Programming Guide](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CoreAnimation_guide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40004514)  を参照してください。）
- UIKitフレームワークの機能を使用して、アニメーション、物理ベースのダイナミクス、およびその他の特殊効果をCocoaTouchユーザーインターフェイスに追加できます。


## OpenGL ES Is a Platform-Neutral API Implemented in iOS

> OpenGL ESは、iOSに実装されたプラットフォームに依存しないAPIです


OpenGL ESはCベースのAPIであるため、非常に移植性が高く、広くサポートされています。C APIとして、Objective-C CocoaTouchアプリとシームレスに統合されます。OpenGL ES仕様では、ウィンドウレイヤーは定義されていません。代わりに、ホスティングオペレーティングシステムは、コマンドを受け入れるOpenGL ESレンダリングコンテキストを作成する関数と、描画コマンドの結果が書き込まれるフレームバッファーを提供する必要があります。iOSでOpenGLESを使用するには、iOSクラスを使用して描画面を設定および表示し、プラットフォームに依存しないAPIを使用してそのコンテンツをレンダリングする必要があります。


**関連する章：** 


## GLKit Provides a Drawing Surface and Animation Support

> GLKitは描画面とアニメーションのサポートを提供します


UIKitフレームワークによって定義されたビューとビューコントローラーは、iOSでのビジュアルコンテンツの表示を制御します。  GLKitフレームワークは、これらのクラスのOpenGLES対応バージョンを提供します。OpenGL ESアプリを開発するときは、GLKViewオブジェクトを使用してOpenGLESコンテンツをレンダリングします。GLKViewControllerオブジェクトを使用して、ビューを管理し、そのコンテンツのアニメーション化をサポートすることもできます。


**関連する章：** 


### iOS Supports Alternative Rendering Targets

> iOSは代替レンダリングターゲットをサポートします


UIKitフレームワークによって定義されたビューとビューコントローラーは、iOSでのビジュアルコンテンツの表示を制御します。  GLKitフレームワークは、これらのクラスのOpenGLES対応バージョンを提供します。  OpenGL ESアプリを開発するときは、GLKViewオブジェクトを使用してOpenGLESコンテンツをレンダリングします。  GLKViewControllerオブジェクトを使用して、ビューを管理し、そのコンテンツのアニメーション化をサポートすることもできます。


## Apps Require Additional Performance Tuning

> アプリには追加のパフォーマンス調整が必要


グラフィックプロセッサは、グラフィック操作用に最適化された並列化されたデバイスです。アプリで優れたパフォーマンスを得るには、データとコマンドをOpenGL ESにフィードするようにアプリを慎重に設計して、グラフィックスハードウェアがアプリと並行して実行されるようにする必要があります。調整が不十分なアプリでは、CPUまたはGPUのいずれかが、もう一方がコマンドの処理を終了するのを待つように強制されます。


OpenGL ESAPIを効率的に使用するようにアプリを設計する必要があります。アプリの作成が完了したら、Instrumentsを使用してアプリのパフォーマンスを微調整します。アプリがOpenGLES内でボトルネックになっている場合は、このガイドに記載されている情報を使用して、アプリのパフォーマンスを最適化してください。


Xcodeは、OpenGLESアプリのパフォーマンスを向上させるのに役立つツールを提供します。


## OpenGL ES May Not Be Used in Background Apps

> OpenGLESはバックグラウンドアプリでは使用できない場合があります


バックグラウンドで実行されているアプリは、OpenGLES関数を呼び出さない場合があります。アプリがバックグラウンドでグラフィックプロセッサにアクセスすると、iOSによって自動的に終了します。これを回避するには、アプリは、バックグラウンドに移動する前にOpenGL ESに以前に送信された保留中のコマンドをフラッシュし、フォアグラウンドに戻るまでOpenGLESを呼び出さないようにする必要があります。


## OpenGL ES Places Additional Restrictions on Multithreaded Apps

> OpenGL ESは、マルチスレッドアプリに追加の制限を課します

同時実行性を利用するようにアプリを設計すると、アプリのパフォーマンスを向上させるのに役立ちます。OpenGL ESアプリに同時実行性を追加する場合は、2つの異なるスレッドから同時に同じコンテキストにアクセスしないようにする必要があります。


## How to Use This Document

> このドキュメントの使用方法


最初の3つの章を読むことから始めます：iOS用のOpenGL ESアプリを構築するためのチェックリスト、OpenGL ESコンテキストの構成、OpenGLESおよびGLKitを使用した描画。これらの章では、OpenGL ESがiOSに統合される方法の概要と、最初のOpenGLESアプリをiOSデバイスで起動して実行するために必要なすべての詳細を提供します。



iOSでのOpenGLESの使用の基本に精通している場合は、プラットフォーム固有の重要なガイドラインについて、「他のレンダリング先への描画とマルチタスク、高解像度、およびその他のiOS機能」をお読みください。5.0より前のiOSバージョンでのOpenGLESの使用に精通している開発者は、OpenGL ES開発を合理化するための新機能の詳細について、OpenGLESおよびGLKitを使用した描画を学習する必要があります。


最後に、OpenGL ES設計ガイドライン、OpenGL ESアプリの調整、および次の章を読んで、効率的なOpenGLESアプリを設計する方法をさらに深く掘り下げます。


特に明記されていない限り、この本のOpenGLESコード例はOpenGLES3.0を対象としています。これらのコード例を他のOpenGLESバージョンで使用するには、変更が必要になる場合があります。


## Prerequisites

> 前提条件


OpenGL ESを使用する前に、一般的なiOSアプリのアーキテクチャに精通している必要があります。 今すぐiOSアプリの開発を開始する（廃止）を参照してください。


このドキュメントは、クロスプラットフォームのOpenGL ESAPIの完全なチュートリアルまたはリファレンスではありません。OpenGL ESの詳細については、以下のリファレンスを参照してください。


## See Also

> 関連項目


OpenGL ESは、クロノスグループによって定義されたオープンスタンダードです。OpenGL ES標準の詳細については、[http://www.khronos.org/opengles/](http://www.khronos.org/opengles/)のWebページを参照してください。


- Addison-Wesleyによって発行されたOpenGL®ES3.0プログラミングガイドは、OpenGLESの概念の包括的な紹介を提供します。
- 同じくAddison-Wesleyによって公開されているOpenGL®ShadingLanguage、Third Editionは、OpenGLESアプリで使用できる多くのシェーディングアルゴリズムを提供します。 モバイルグラフィックプロセッサで効率的に実行するには、これらのアルゴリズムの一部を変更する必要がある場合があります。
- OpenGL ES APIレジストリは、OpenGL ES仕様、OpenGL ESシェーディング言語仕様、およびOpenGLES拡張機能のドキュメントの公式リポジトリです。
- OpenGL ESフレームワークリファレンスでは、OpenGLESをiOSに統合するためにAppleが提供するプラットフォーム固有の関数とクラスについて説明しています。
- iOSデバイス互換性リファレンスには、アプリで利用できるハードウェアとソフトウェアの機能に関する詳細情報が記載されています。
- GLKitフレームワークリファレンスでは、OpenGL ES2.0および3.0アプリの開発を容易にするためにAppleが提供するフレームワークについて説明しています。

