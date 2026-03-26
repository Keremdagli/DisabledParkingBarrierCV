# Disabled Parking Barrier CV — YOLO11 Tabanlı Nesne Tespiti

Bu repo, **engelli park bariyerleri tespiti** problemine yönelik bir **bilgisayarlı görü** çalışmasının eğitim çıktılarının (metrikler, grafikler, örnek tahmin görselleri ve ağırlıklar) derli toplu bir özetini içerir.

## TÜBİTAK Desteği

Bu çalışma **TÜBİTAK 2209-A** kapsamında, **2023/2 dönemi** desteğiyle yürütülmüştür.

## Problem Tanımı

Amaç, görüntüler üzerinde engelli park alanları/bariyerleri senaryosunda kritik görsel işaretleri algılayarak (nesne tespiti) otomatik bir karar/veri akışına temel oluşturmaktır.

> Not: Bu klasör eğitim “çıktıları” odaklıdır. Dataset tanımı (`data.yaml`) bu klasörde bulunmadığı için sınıf isimleri eğitim artefaktlarından otomatik doğrulanamıyor. README boyunca sınıfları proje bağlamına uygun şekilde **“plaka”** ve **“engelli sembolü”** olarak ele alıyoruz.

## Dataset (2 Sınıf)

Model sadece aşağıdaki 2 sınıfı tespit edecek şekilde eğitilmiştir:

- **Plaka**
- **Engelli sembolü**

## Model ve Eğitim Ayarları

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

## Sonuçlar (Özet Metrikler)

Metrikler `results.csv` üzerinden alınmıştır.

### En iyi mAP50-95 (daha “zor” ve genelleme odaklı metrik)

- **Epoch**: 96  
- **Precision**: 0.98839  
- **Recall**: 0.95112  
- **mAP50**: 0.97957  
- **mAP50-95**: 0.65803  

### En iyi mAP50

- **Epoch**: 108  
- **Precision**: 0.99446  
- **Recall**: 0.98810  
- **mAP50**: 0.99477  
- **mAP50-95 (aynı epoch)**: 0.54546  

### Eğitim sonu (son kaydedilen epoch)

- **Epoch**: 116  
- **Precision**: 0.99387  
- **Recall**: 0.98452  
- **mAP50**: 0.99477  
- **mAP50-95**: 0.60826  

## Eğitim Eğrileri ve Grafikler

### Genel sonuç grafiği

`results.png`, epoch boyunca loss ve metrik trendlerini özetler.

![Training results summary](results.png)

### Precision / Recall / F1 / PR eğrileri

- `P_curve.png`: “plaka” ve “engelli sembolü” sınıflarında (ve genel olarak) precision davranışı
- `R_curve.png`: recall davranışı
- `F1_curve.png`: F1 skoru davranışı (precision–recall dengesi)
- `PR_curve.png`: Precision-Recall eğrisi (eşik değiştikçe trade-off)

![Precision curve](P_curve.png)
![Recall curve](R_curve.png)
![F1 curve](F1_curve.png)
![PR curve](PR_curve.png)

## Değerlendirme Artefaktları

### Confusion matrix

- `confusion_matrix.png`: ham confusion matrix
- `confusion_matrix_normalized.png`: **normalized confusion matrix**, sınıflar arası karışmaları daha net görselleştirir (ör. **plaka** ↔ **engelli sembolü** yanlış sınıflandırmaları).

![Confusion matrix](confusion_matrix.png)
![Normalized confusion matrix](confusion_matrix_normalized.png)

### Dataset etiket istatistikleri

- `labels.jpg`: etiket dağılımı / örnekler özeti
- `labels_correlogram.jpg`: etiket korelasyon grafiği (dataset yapısı hakkında ipucu)

![Labels summary](labels.jpg)
![Labels correlogram](labels_correlogram.jpg)

## Görsel Demo (Validation Tahmin Örnekleri)

Aşağıdaki görseller, aynı validation batch’i için ground-truth ve model tahminlerini yan yana incelemeye uygundur.

### Batch 0

- Ground-truth: `val_batch0_labels.jpg`
- Prediction: `val_batch0_pred.jpg`

![Val batch 0 — labels](val_batch0_labels.jpg)
![Val batch 0 — predictions](val_batch0_pred.jpg)

### Batch 1

- Ground-truth: `val_batch1_labels.jpg`
- Prediction: `val_batch1_pred.jpg`

![Val batch 1 — labels](val_batch1_labels.jpg)
![Val batch 1 — predictions](val_batch1_pred.jpg)

### Batch 2

- Ground-truth: `val_batch2_labels.jpg`
- Prediction: `val_batch2_pred.jpg`

![Val batch 2 — labels](val_batch2_labels.jpg)
![Val batch 2 — predictions](val_batch2_pred.jpg)

## Eğitimden Örnekler (Train Batches)

Eğitim sırasında kullanılan batch örnekleri:

![Train batch 0](train_batch0.jpg)
![Train batch 1](train_batch1.jpg)
![Train batch 2](train_batch2.jpg)

## Dosyalar ve Ne İşe Yarar?

- `args.yaml`: eğitim parametreleri
- `results.csv`: epoch bazlı metrikler + train/val loss
- `events.out.tfevents.*`: TensorBoard log’u
- `weights/best.pt`: en iyi checkpoint
- `weights/last.pt`: son checkpoint

## TensorBoard (Opsiyonel)

TensorBoard ile logları görüntülemek için bu klasörde:

```bash
tensorboard --logdir .
```

> Windows/CPU/GPU ortamına göre TensorBoard kurulumu değişebilir.

## Ağırlıklar (Inference için)

- `weights/best.pt` genelde demo/inference için önerilir (en iyi checkpoint).
- `weights/last.pt` eğitim sonu durumunu temsil eder.

## Lisans / Atıf

Bu çalışma TÜBİTAK 2209-A (2023/2) kapsamında desteklenmiştir. Akademik/kurumsal kullanımda uygun atıf verilmesi önerilir.

