# Bass Tone Shaping Strategy for Rock & Funk

## 1. Bass Setup (Instrument Settings)
ベース本体の物理的なセッティングは、音作りの土台となる最も重要な要素です。ジャンルごとの奏法（ロックのピック弾き・指弾き、ファンクのスラップ）に合わせて調整します。

### String Action (弦高)
**Claim**: ロックは低めのアクション、ファンク（特にスラップ）はやや高めのアクションが推奨される。
- **Rock**: 12フレット上で **2.0mm 〜 2.5mm**。
    - 理由: 速いパッセージやピック弾きに対応しやすく、フレットに弦が当たる「バズ」を含んだ攻撃的な金属音（Grind）を出しやすい。
- **Funk**: 12フレット上で **2.5mm 〜 3.0mm**。
    - 理由: スラップ（Thumbing/Plucking）時の弦の振幅を確保するため。弦高が低すぎると、弦がフレットに当たりすぎてサステインが失われ、音が詰まる原因となる。

**Source**:
- [technical_manual/tier2] [Fender Bass Setup Guide](https://support.fender.com/en-us/knowledgebase/article/KA-01903) (unknown) — primary_source: true
- [community_data/tier3] [Bass Guitar Setup Guide for Slap](https://www.ultimatebassguitar.com/bass-action-height-guide/) (2024) — primary_source: false
**Confidence**: high

### Pickup Height & Selection (ピックアップ調整)
**Claim**: ピックアップの高さとバランスで出力とトーンのキャラクターを決定する。
- **Height**: 最終フレットを押さえた状態で、ポールピースと弦の間隔を **2.5mm 〜 3.0mm (Bass側) / 2.0mm (Treble側)** に設定するのが標準。
    - ロック: 標準より少し高めにし、出力を稼ぎアンプをドライブさせやすくする（ただし磁力による弦振動の阻害に注意）。
    - ファンク: クリアさを重視し標準または少し低めに設定。特にスラップ時のピークで音が潰れないようにする。
- **Selection**:
    - ロック: プレシジョンベース（P-Bass）タイプ、または両方のPUをフルアップにして中低域の厚みを出す。
    - ファンク: ジャズベース（J-Bass）タイプで両PUフル（ドンシャリ傾向）か、リア（Bridge）PU寄りで粒立ちの良い音を作る。

**Source**:
- [community_data/tier3] [How to Set Pickup Height](https://www.guitarworld.com/lessons/how-to-set-bass-pickups-for-optimum-tone) (2023) — primary_source: false
**Confidence**: medium

## 2. Amp EQ Settings (アンプ設定)
アンプのEQは「全てのツマミを12時（フラット）」から開始し、ジャンルに合わせてカット＆ブーストを行います。

### Rock EQ Strategy
**Claim**: 中音域（Mids）を強調し、アンサンブルの中で埋もれない「太い」音を作る。
- **Bass (Low)**: 12時〜1時（Boost）。バスドラムと帯域が被りすぎないよう上げすぎに注意。
- **Mids**: 1時〜2時（Boost）。ロックベースの核となる帯域。ギターの壁を突き抜けるために必須。
    - Low-Mid（もしあれば）を上げると「ゴリッ」としたロック特有の唸りが出る。
- **Treble (High)**: 12時〜1時（Slight Boost）。ピックのアタック音や指のタッチを明確にする。

**Source**:
- [community_data/tier3] [Rock Bass EQ Settings](https://soundshockaudio.com/how-to-eq-rock-bass/) (2023) — primary_source: false
**Confidence**: medium

### Funk EQ Strategy
**Claim**: いわゆる「ドンシャリ（Scooped Mids）」傾向だが、過度なカットは禁物。
- **Bass (Low)**: 1時〜2時（Boost）。重厚なボトムエンドを確保。
- **Mids**: 10時〜11時（Cut）。スラップの「バキッ」とした金属的な響き（High）と「ドン」という低音（Low）を強調するため、中域を少し削る。
    - *注意*: 指弾きファンク（Fingerstyle Funk）の場合は、逆にMidをブーストして「ブリブリ」した音（Tower of Power風）を作る場合もある。
- **Treble (High)**: 2時〜3時（Boost）。プル（Plucking）の鋭さと煌びやかさを出すために重要。

**Source**:
- [community_data/tier3] [Bass Amp Settings for Funk](https://allforturntables.com/2023/06/22/bass-amp-settings-for-funk/) (2023) — primary_source: false
**Confidence**: medium

## 3. Effects & Pedals (エフェクター活用)
初心者が最初に揃えるべきエフェクターと、ジャンル別のセッティング例です。

### Compressor (必須: Rock & Funk)
**Claim**: 音の粒を揃え、サステインを稼ぐ。特にスラップ奏法ではピーク（突発的な大音量）を抑えるために不可欠。
- **Settings (Funk/Slap)**:
    - **Ratio**: 4:1 〜 6:1（強めの圧縮）
    - **Attack**: Medium-Slow（15-30ms）。アタック音（スラップの「バチッ」という瞬間）を潰さないよう、コンプがかかるのを少し遅らせる。
    - **Release**: Medium-Fast（50-100ms）。次の音に影響しないよう素早く戻す。
- **Settings (Rock)**:
    - **Ratio**: 4:1
    - **Attack**: Fast（10-20ms）。音の粒を均一にし、安定したボトムを支える。

**Source**:
- [community_data/tier3] [Bass Compression Guide](https://homestudioguys.com/blog/understanding-bass-guitar-compression-and-its-uses/) (2023) — primary_source: false
**Confidence**: high

### Envelope Filter / Auto-Wah (Funk)
**Claim**: ファンク特有の「ワウワウ」「ケロケロ」という母音のようなサウンドを作る。
- **活用法**: ブーツィー・コリンズやフリー（RHCP）のようなファンクベースに必須。ピッキングの強弱に反応してフィルターが開閉する。
- **Settings**:
    - **Sensitivity**: 演奏のダイナミクスに合わせて調整（強く弾いた時にフィルターが全開になる位置）。
    - **Decay**: 短め（Funk）に設定し、リズミカルな「クワッ」という音を作る。

**Source**:
- [community_data/tier3] [Best Bass Pedals for Funk](https://www.musicindustryhowto.com/5-best-bass-pedals-for-funk-metal-rock-other-guitarists-and-what-bassists-should-look-for-when-buying/) (2024) — primary_source: false
**Confidence**: medium

### Overdrive / Preamp (Rock)
**Claim**: チューブアンプを大音量で鳴らしたような「歪み（Saturation）」を加え、音に倍音と攻撃性を付加する。
- **活用法**: 常にONにして基本の音色（Base Tone）とする使い方が一般的（サンズアンプ等）。
- **Settings**:
    - **Drive/Gain**: 9時〜10時（Low Gain）。激しく歪ませるのではなく、強く弾いた時だけ少し歪む程度がアンサンブルに馴染む。

**Source**:
- [community_data/tier3] [Essential Bass Pedals](https://guitarstrive.com/essential-bass-pedals/) (2023) — primary_source: false
**Confidence**: medium

## 4. Recommended Gear for Beginners (機材例)
入手しやすく、プロの利用者も多い「業界標準」的な機材です。

| Category | Recommended Model | Description |
| :--- | :--- | :--- |
| **Compressor** | **MXR M87 Bass Compressor** | 視覚的にリダクション量が見え、設定が直感的。スタジオ品質。 |
| **Compressor** | **Boss BC-1X** | マルチバンドコンプ。ノイズが少なく、自然なコンプレッションが得られる。 |
| **Env Filter** | **MXR M82 Bass Envelope Filter** | 原音（Dry）とエフェクト音（FX）をミックスでき、低音痩せを防げるファンク定番。 |
| **Preamp/DI** | **Tech 21 SansAmp Bass Driver DI** | ロックベースの「あの音」を一発で作る業界標準。歪みとEQ補正に最適。 |
| **Preamp/DI** | **Boss BB-1X** | サンズアンプよりモダンでクリアな歪み。スラップの抜けも良い。 |

**Source**:
- [community_data/tier4] [TalkBass Forum Consensus](https://www.talkbass.com/) (Various threads) — primary_source: false
**Confidence**: medium

## 5. Conclusion & Recommendations
- **Rock**: 「Mid Boost」と「Low Action」で攻撃性とスピードを重視。SansAmpのようなプリアンプで少し歪ませるのが近道。
- **Funk**: 「Scooped Mid (Slight)」と「High Action (Relatively)」でクリアさとダイナミクスを重視。コンプレッサーの設定（Attackを遅くする）がスラップサウンドの肝。
- **Priority**: まずはベース本体の調整（弦高）とコンプレッサーの導入から始めることを強く推奨します。これだけで弾きやすさと音のまとまりが劇的に改善します。
