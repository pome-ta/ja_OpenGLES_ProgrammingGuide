# ja_OpenGLES_ProgrammingGuide

[OpenGL ES Programming Guide](https://developer.apple.com/library/archive/documentation/3DDrawing/Conceptual/OpenGLES_ProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40008793) の日本語機械翻訳


内容を読みながら、実装確認をしていく



# About OpenGL ES

> 重要：OpenGLESはiOS12で非推奨になりました。GPUで高性能コードを作成するには、代わりにMetalフレームワークを使用してください。 [Metal](https://developer.apple.com/metal/)を参照してください。


Open Graphics Library（OpenGL）は、2Dおよび3Dデータを視覚化するために使用されます。 これは、2Dおよび3Dデジタルコンテンツ作成、機械および建築設計、仮想プロトタイピング、飛行シミュレーション、ビデオゲームなどのアプリケーションをサポートする多目的オープンスタンダードグラフィックライブラリです。OpenGLを使用して、3Dグラフィックスパイプラインを構成し、それにデータを送信します。頂点は変換および照明され、プリミティブにアセンブルされ、ラスタライズされて2Dイメージが作成されます。OpenGLは、関数呼び出しを、基盤となるグラフィックハードウェアに送信できるグラフィックコマンドに変換するように設計されています。 この基盤となるハードウェアはグラフィックコマンドの処理専用であるため、OpenGLの描画は通常非常に高速です。


OpenGL for Embedded Systems（OpenGL ES）は、OpenGLの簡略化されたバージョンであり、冗長な機能を排除して、モバイルグラフィックスハードウェアでの学習と実装の両方が容易なライブラリを提供します。


[画像リンク](#)


## At a Glance

OpenGL ESを使用すると、アプリは基盤となるグラフィックプロセッサの能力を活用できます。iOSデバイスのGPUは、高度な2Dおよび3D描画を実行できるだけでなく、最終画像のすべてのピクセルに対して複雑なシェーディング計算を実行できます。 アプリの設計要件でGPUハードウェアへの可能な限り最も直接的で包括的なアクセスが必要な場合は、OpenGLESを使用する必要があります。OpenGL ESの一般的なクライアントには、3Dグラフィックスを表示するビデオゲームやシミュレーションが含まれます。



