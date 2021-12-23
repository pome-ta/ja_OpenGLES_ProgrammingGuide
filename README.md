# ja_OpenGLES_ProgrammingGuide

[OpenGL ES Programming Guide](https://developer.apple.com/library/archive/documentation/3DDrawing/Conceptual/OpenGLES_ProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40008793) の日本語機械翻訳


内容を読みながら、実装確認をしていく



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
