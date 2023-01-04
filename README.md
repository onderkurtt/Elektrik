# Bu çalışmada nesne takibi ile tesislerde kullanılan elektrik ekipmanlarının detection işlemini yaptık

|<p>İlk olarak Yolov7’ninmdökümanlarına ulaşmamız gerekmektedir. Aşağıdaki git adresi üzerinden dosyaların colab ortamına aktarmayı sağlıyoruz. </p><p></p><p>%cd                                 ----------------------- ile yolov7 klasörü içine giriyoruz</p><p>!pip install -r requirements.txt ------------ ile çalıştırmak için gerekli araçları colab ortamına yüklüyoruz</p><p></p>|
| :- |

|<p>!git clone https://github.com/WongKinYiu/yolov7.git</p><p>%cd yolov7</p><p>!pip install -r requirements.txt</p>|
| :- |

||
| :- |



|<p>Başarılı bir şekilde dosyalar indirildikten sonra Sağ tarafta klasör simgesinde indirilen dosyaları görebiliriz. Daha önceden etiketleme yapmış olduğum siteden verileri çekmek için ;</p><p>` `<https://app.roboflow.com/mustafakemalatatrk/pano-4t3fv/1></p><p>adresine gidip colabda çalıştırmak üzere kod satırını alıyoruz. Aşağıdaki kod satırı roboflow üzerinden otomatik alınmaktadır.</p><p></p>|
| :- |

|<p>!pip install roboflow</p><p></p><p>from roboflow import Roboflow</p><p>rf = Roboflow(api\_key="xdRfzsQl2E0NSbZQ49Be")</p><p>project = rf.workspace("mustafakemalatatrk").project("pano-4t3fv")</p><p>dataset = project.version(1).download("yolov7")</p>|
| :- |

||
| :- |



|<p>“Dosyaları çekip çalıştırdıktan sonra “Restart Runtime” yaparak çalışmamıza devam ediyoruz. Yolov7 eğitimine devam etmemiz için yolov7 klasörünün kendi kodlarını çalıştırıyoruz. Yolov7 klasörü içindeki yolov7.pt dosyasını indiriyoruz.</p><p></p>|
| :- |

|<p>%cd /content/yolov7</p><p>!wget "https://github.com/WongKinYiu/yolov7/releases/download/v0.1/yolov7.pt"</p>|
| :- |

||
| :- |





Yolov7.pt colab ortamına eklendikten sonra eğitime başlayabiliriz. Kod satırı içinde ;

|<p>` `%cd /content/yolov7</p><p>!python train.py --batch 16 --cfg cfg/training/yolov7.yaml --epochs 325 --data {dataset.location}/data.yaml --weights 'yolov7.pt' --device 0</p><p></p>|
| :- |

|<p>!python train.py                                    yolov7 klasörü içindeki train.py dosyasını çalıştır.             </p><p>batch 16                                                 size 16 olarak ata.</p><p>cfg/training/yolov7.yaml                    yolov7 nin hangi veri türünü kullanacağını belirtir.</p><p>epochs  325                                           eğitim gerçekleştirilecek sayı</p><p>weights 'yolov7.pt'                               kullanılacak ağırlık</p>|
| :- |

||
| :- |

Colab yolov7 bir eğitim başlatır. Verdiğimiz parametrelerle birlikte train.py içerisinde bulunan parametrelerle 325 epochluk eğitim gerçekleşecek. İmage 640x640 İlk epoch sonrasında mean average precision(mAP) değeri 0,00562 oldu. Bu değer epoch sayısı arttıkça artacaktır.

Eğitim sonrasında ;

|<p>last.pt ve best.pt adında iki tane model oluşturuldu.</p><p>last.pt   --       son epoch üzerindeki değerleri kullanarak oluşturuldu. </p><p>best.pt --       325 adımdaki en iyi mAP noktasına ulaştığında kaydettiği değerler.</p>|
| :- |



|<p>Grafik halinde görmek için tensorboard kodu çalıştırıldı.</p><p>%load\_ext tensorboard</p><p>%tensorboard --logdir runs/train</p>|
| :- |

eğittiğimiz modeli kullanmak adına , model üzerinden test için !python detect.py --weights eğitilmiş model bekliyor.Bu modeli yukarıda oluşturduğumuz best.pt olarak kullanacağız.

!python detect.py --weights/content/yolov7/runs/train/exp/weights/best.pt

!python detect.py --weights /content/yolov7/runs/train/exp/weights/best.pt --conf 0.5 --source /content/9.jpeg

--conf değeri evolutionda kullanacağımız değer 0.5 üzerinde her hangi bir şey bulursa bizim nesnemiz olarak algılayacak.

source kısmı için sol tarafa tanımlamak istediğimiz bir resim( 9.jpeg) yükleyip adres kaynağını yazdık.

-----
best.pt modelini kullanarak verdiğimiz source kaynaktaki nesneyi tespit etmeye çalışıyor. 1 role, 3 sigorta, 1 termik, Done. (15.1ms) Inference, (1.4ms) NMS The image with the result is saved in: runs/detect/exp11/9.jpeg Done. (0.204s)

![](Aspose.Words.3ae9d2ae-1da9-4ca2-9bec-b3087eb2ed00.001.png)


