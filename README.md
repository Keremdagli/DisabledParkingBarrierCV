<h1>Disabled Parking Barrier CV — YOLO11 Tabanlı Nesne Tespiti</h1>

Bu repo, **engelli park bariyerleri tespiti** problemine yönelik bir **bilgisayarlı görü** çalışmasının eğitim çıktılarının (metrikler, grafikler, örnek tahmin görselleri ve ağırlıklar) derli toplu bir özetini içerir.

<h2>TÜBİTAK Desteği</h2>

Bu çalışma **TÜBİTAK 2209-A** kapsamında, **2023/2 dönemi** desteğiyle yürütülmüştür.

<h2>Problem Tanımı</h2>

Amaç, görüntüler üzerinde engelli park alanları/bariyerleri senaryosunda kritik görsel işaretleri algılayarak (nesne tespiti) otomatik bir karar/veri akışına temel oluşturmaktır.

> Not: Bu klasör eğitim “çıktıları” odaklıdır. Dataset tanımı (`data.yaml`) bu klasörde bulunmadığı için sınıf isimleri eğitim artefaktlarından otomatik doğrulanamıyor. README boyunca sınıfları proje bağlamına uygun şekilde **“plaka”** ve **“engelli sembolü”** olarak ele alıyoruz.

<h2>Dataset (2 Sınıf)</h2>

Model sadece aşağıdaki 2 sınıfı tespit edecek şekilde eğitilmiştir:

- **Plaka**
- **Engelli sembolü**

<h2>Model ve Eğitim Ayarları</h2>

Eğitim konfigi `args.yaml` dosyasından özetlenmiştir:

- **Task**: `detect`
- **Mode**: `train`
- **Mimari**: `yolo11m.yaml` (Ultralytics YOLO11)
- **Epochs (hedef)**: `200`
- **Patience**: `20` (erken durdurma)
- **Batch**: `8`
- **Image size**: `640`
- **Pretrained**: `true`
- **Optimizer**: `auto`
- **IoU (eval ayarı)**: `0.7`
- **AMP**: `true`
- **Seed**: `0` / **Deterministic**: `true`
- **Save dir**: `runs/detect/train`

<h2>Sonuçlar (Özet Metrikler)</h2>

Metrikler `results.csv` üzerinden alınmıştır.

<h3>En iyi mAP50-95 (daha “zor” ve genelleme odaklı metrik)</h3>

- **Epoch**: 96  
- **Precision**: 0.98839  
- **Recall**: 0.95112  
- **mAP50**: 0.97957  
- **mAP50-95**: 0.65803  

<h3>En iyi mAP50</h3>

- **Epoch**: 108  
- **Precision**: 0.99446  
- **Recall**: 0.98810  
- **mAP50**: 0.99477  
- **mAP50-95 (aynı epoch)**: 0.54546  

<h3>Eğitim sonu (son kaydedilen epoch)</h3>

- **Epoch**: 116  
- **Precision**: 0.99387  
- **Recall**: 0.98452  
- **mAP50**: 0.99477  
- **mAP50-95**: 0.60826  

<h2>Eğitim Eğrileri ve Grafikler</h2>

<h3>Genel sonuç grafiği</h3>

`results.png`, epoch boyunca loss ve metrik trendlerini özetler.

<img src="train/results.png" alt="Training results summary" width="50%">

<h3>Precision / Recall / F1 / PR eğrileri</h3>

- `P_curve.png`: “plaka” ve “engelli sembolü” sınıflarında (ve genel olarak) precision davranışı
- `R_curve.png`: recall davranışı
- `F1_curve.png`: F1 skoru davranışı (precision–recall dengesi)
- `PR_curve.png`: Precision-Recall eğrisi (eşik değiştikçe trade-off)

<img src="train/P_curve.png" alt="Precision curve" width="50%">
<img src="train/R_curve.png" alt="Recall curve" width="50%">
<img src="train/F1_curve.png" alt="F1 curve" width="50%">
<img src="train/PR_curve.png" alt="PR curve" width="50%">

<h2>Değerlendirme Artefaktları</h2>

<h3>Confusion matrix</h3>

- `confusion_matrix.png`: ham confusion matrix
- `confusion_matrix_normalized.png`: **normalized confusion matrix**, sınıflar arası karışmaları daha net görselleştirir (ör. **plaka** ↔ **engelli sembolü** yanlış sınıflandırmaları).

<img src="train/confusion_matrix.png" alt="Confusion matrix" width="50%">
<img src="train/confusion_matrix_normalized.png" alt="Normalized confusion matrix" width="50%">

<h3>Dataset etiket istatistikleri</h3>

- `labels.jpg`: etiket dağılımı / örnekler özeti
- `labels_correlogram.jpg`: etiket korelasyon grafiği (dataset yapısı hakkında ipucu)

<img src="train/labels.jpg" alt="Labels summary" width="50%">
<img src="train/labels_correlogram.jpg" alt="Labels correlogram" width="50%">

<h2>Görsel Demo (Validation Tahmin Örnekleri)</h2>

Aşağıdaki görseller, aynı validation batch’i için ground-truth ve model tahminlerini yan yana incelemeye uygundur.

<h3>Batch 0</h3>

- Ground-truth: `val_batch0_labels.jpg`
- Prediction: `val_batch0_pred.jpg`

<img src="train/val_batch0_labels.jpg" alt="Val batch 0 — labels (ground-truth)" width="50%">
<img src="train/val_batch0_pred.jpg" alt="Val batch 0 — predictions" width="50%">

<h3>Batch 1</h3>

- Ground-truth: `val_batch1_labels.jpg`
- Prediction: `val_batch1_pred.jpg`

<img src="train/val_batch1_labels.jpg" alt="Val batch 1 — labels (ground-truth)" width="50%">
<img src="train/val_batch1_pred.jpg" alt="Val batch 1 — predictions" width="50%">

<h3>Batch 2</h3>

- Ground-truth: `val_batch2_labels.jpg`
- Prediction: `val_batch2_pred.jpg`

<img src="train/val_batch2_labels.jpg" alt="Val batch 2 — labels (ground-truth)" width="50%">
<img src="train/val_batch2_pred.jpg" alt="Val batch 2 — predictions" width="50%">

<h2>Eğitimden Örnekler (Train Batches)</h2>

Eğitim sırasında kullanılan batch örnekleri:

<img src="train/train_batch0.jpg" alt="Train batch 0" width="50%">
<img src="train/train_batch1.jpg" alt="Train batch 1" width="50%">
<img src="train/train_batch2.jpg" alt="Train batch 2" width="50%">

<h2>Dosyalar ve Ne İşe Yarar?</h2>

- `args.yaml`: eğitim parametreleri
- `results.csv`: epoch bazlı metrikler + train/val loss
- `events.out.tfevents.*`: TensorBoard log’u
- `weights/best.pt`: en iyi checkpoint
- `weights/last.pt`: son checkpoint

<h2>TensorBoard (Opsiyonel)</h2>

TensorBoard ile logları görüntülemek için bu klasörde:

```bash
tensorboard --logdir .
```

> Windows/CPU/GPU ortamına göre TensorBoard kurulumu değişebilir.

<h2>Ağırlıklar (Inference için)</h2>

- `weights/best.pt` genelde demo/inference için önerilir (en iyi checkpoint).
- `weights/last.pt` eğitim sonu durumunu temsil eder.

<h2>Lisans / Atıf</h2>

Bu çalışma TÜBİTAK 2209-A (2023/2) kapsamında desteklenmiştir. Akademik/kurumsal kullanımda uygun atıf verilmesi önerilir.

