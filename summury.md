初めてLightGBMの論文ちゃんと読んだのでメモ。
XGBoostに対して、GOSSとEFBが更新点。

GOSSは勾配が大きいデータは残し、小さいデータは減らし、元データと同じ比率になるよう小さいデータを乗ずることで計算効率向上。

EFBは疎な特徴量を効率よく計算する方法。疎な特徴量は大体exclusiveな(同じ行で１が立たない)ことが分かっているから、それらを一つにまとめる。ただし、完全に被らないようにするのはNP困難なため、ある程度の被りは許す。

実験結果のざっくりまとめは、
【速度】
lgb(histogram-based,EFB,GOSS)
>lgb(histogram-based,EFB)
>xgb(histogram-based)
>lgb(histogram-based)
>xgb(pre-sorted)

【精度】
ほぼ一緒。
多少 leaf-wise > layer-wise(depth)。

感想として、
・GOSSの方が早くて精度も良い、と論文内ではなってるけど、実装だと"boosting_type"で反映させる項目で、ここみんなgbdtにしてるよなぁ
・「LightGBMとはなんぞや」ブログとかだと、histogram-based、leaf-wiseが取り上げられることが多いけれど、論文内で取り上げているのはGOSSとEFBメインなんだなぁという感じ。GOSS切っててもXGBoostよりも早いから、多分EFBがすごいアルゴリズムなのだと思う。