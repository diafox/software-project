## Generovanie outfitov pomocou evolučného algoritmu ##
1. **Definovanie cieľov algoritmu** (čo chceme optimalizovať): 
- _personalizácia_ - ako veľmi vygenerovaný outfit zodpovedá uživateľovým preferenciám, 
- _rozmanitosť_ - či je outfit dostatočne rozmanitý, 
- _kompatibilita_ - či má outfit koherentný štýl a farebnosť (môžeme využiť O’Donovan-ov Color Compatibility Predictor), 
- _sezónnosť_ - či je outfit vhodný pre konkrétnu sezónu (ročné obdobie / počasie).
2. **Reprezentácia outfitu**: každý outfit musí byť reprezentovateľný nejakou postupnosťou ‘kúskov’. Jednotlivé oblečenie je reprezentované pomocou vektorov _[kategória, sub-kategória, farba, potisk, materiál, štýl, sezónnosť]_. Každý outfit je reprezentovaný maticou, ktorá sa skladá z týchto vektorov. Každá kategória sa môže vyskytovať iba raz.
- Kategória: vršok 1. vrstva, vršok 2. vrstva, spodok, full-body oblečenie, kabelka, doplnky
- Sub-kategória: tielko, blúzka, rolák, mikina, sveter, bunda, džíny, šortky, sukňa, šaty, overal…
- Potisk: čistý, kvetinový, prúžkovaný, kockovaný, bodkovaný…
- Materiál: šifón, bavlna, denim, čipka, koža, satén, flaner, flitrované, úplet…
- Štýl: elegantné, plážové, casual, business…
- Sezónnosť: letné, zimné, celoročné, prechodné
3. **Definovanie fitness funkcie**: fitness funkcia vyhodnocuje kvalitu každého outfitu na základe definovaných cieľov. V tomto prípade by to bol napríklad priemer jednotlivých cieľových ohodnotení.
4. **Generovanie prvej populácie** (rodičov): prebieha náhodne, avšak je obmedzená heuristikou, napr. kategória sa vyskytuje iba raz, sezónnosť je prepojená s počasím/dátumom, zakázané kombinácie (koženné nohavice a kvetinový sveter, plesové šaty a tenisky, tepláky a sako). Následne je vypočítaná ‘fitness’ každého vygenerovaného outfitu. Uživateľovi je následne ponúknutý určitý počet (napr. 10) outfitov s najväčším fitness.
5. **Ohodnotenie a vznik potomkov**: na základe uživateľovho ohodnotenia sa upraví fitness outfitu (pretože nie každý outfit, ktorého fitness bola vysoká, sa bude aj reálne uživateľovi páčiť). Vyselektujú sa outfity s najvyšším hodnotením a aplikujú sa genetické operátory - kríženie a mutácia, ktorej cieľom je zlepšenie rôznorodosti. Vznikne nová generácia, ktorá bude uživateľovi znova ponúknutá. Generovanie novej generácie sa opakuje po obmedzenom počte uživateľom ohodnotených outfitov (1 pre úspešný outfit, 0 pre neúspešný outfit)

#### [O’Donovan: Color Compatibility From Large Datasets](http://www.dgp.toronto.edu/~donovan/color/colorcomp.pdf) ####


## Získavanie atribútov z fotografií uživateľa ##
#### [MMFashion: An Open-Source Toolbox for Visual Fashion Analysis](https://arxiv.org/pdf/2005.08847.pdf) ####
- comprehensive, flexible and user-friendly open-source visual fashion analysis toolbox based on PyTorch 
- backbone is the part that transforms an image to feature maps, such as a ResNet-50 without the last fully connected layer
- head is the part that takes the features as input and makes task-specific predictions 
- we use the unified training pipeline with hooking mechanism as shown in MMDetection 
- training epochs and validation epochs run iteratively and validation epochs are optional. In each epoch, we forward and backward the model by many iterations. To make the pipeline more flexible and easy to customize, we define a minimum pipeline which just forwards the model repeatedly 
- used dataset: DeepFashion
- Fashion Attribute Prediction: We use the standard top-k recall rate and accuracy to evaluate the prediction model. We assign k highest-ranked attributes and compare the assigned attributes with the ground-truth attributes. We reimplement the method in DeepFashion * - the “landmark pooling” method is to extract and to utilize local feature maps around the landmark l, while the basic “global pooling” simply uses the global features of the whole image
- Fashion Parsing and Segmentation: detection and segmentation performance are measured with mean Average Precision(mAP). We follow Mask- RCNN to construct fashion detection and segmentation framework, using ResNet50-FPN as backbone
- Fashion Compatibility and Recommendation: determining the compatibility relationship of a given fashion outfit; requests to score a candidate outfit in order to determine if they are compatible or not. Performance is evaluated using the area under a receiver operating curve(AUC). We use the method in ** to complete fashion compatibility learning that develops different embedding projection strategies. Higher Fill-in-the-Blank accuracy and higher compatibility AUC means better performance 
- Fashion Recognition and Retrieval
- Fashion Landmark Detection

#### * [DeepFashion: Powering Robust Clothes Recognition and Retrieval with Rich Annotations](https://www.cv-foundation.org/openaccess/content_cvpr_2016/papers/Liu_DeepFashion_Powering_Robust_CVPR_2016_paper.pdf) ####
- large-scale clothes dataset with comprehensive annotations
- contains over 800,000 images, taken under different scenarios including store, street snapshot, and consumer selfies
- 50 categories, 1,000 descriptive attributes, and clothing landmarks
- deep model FashionNet, which learns clothing features by jointly predicting clothing attributes and landmarks 
- handles clothing deformation/occlusion by pooling/gating feature maps upon estimated landmark locations 
- trained in an iterative manner
- the whole system can be learned end-to-end 
- the network structure of FashionNet is similar to VGG-16, except the last convolutional layer, which is carefully design for clothes
- the forward pass of FashionNet consists of three stages:
  - at the first stage, a clothes image is fed into the network and passed through the branch to predict the landmarks’ locations 
  - at the second stage, which is a landmark pooling layer, leading to local features that are invariant to deformations and occlusions of clothes 
  - at the third stage, the global features and the landmark-pooled local features are concatenated together 
- the backward pass back-propagates the errors of four kinds of loss functions in an iterative manner 
- provides Category classification, attribute prediction, in-shop clothes retrieval and consumer-to-shop clothes retrieval

#### ** [M. I. Vasileva: Learning type-aware embeddings for fashion compatibility](https://openaccess.thecvf.com/content_ECCV_2018/papers/Mariya_Vasileva_Learning_Type-Aware_Embeddings_ECCV_2018_paper.pdf) ####
