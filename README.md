# Kart ile ödeme işlemleri
## Wirecard Nedir?
Wirecard internet üzerinden yapılacak alışverişlerde kredi kartı ile ödeme yapılmasını sağlayan pos servisidir. Buna benzer bankaların da sunduğu servisler vardır. 

Web sitenizde satış yapmak isterseniz bu servisleri kullanabilirsiniz. Yapmanız gereken 
[Wirecard](https://www.wirecard.com.tr/) 'ın sitesine girerek sitenin sunduğu servisi ücret karşılığı alabilirsiniz. Alma işleminden  sonra nasıl sitenize entegre olacağını anlatacam.

## Başlayalım..
İlk olarak [Wirecard](https://www.wirecard.com.tr/) 'ın sitsine girelim. Geliştirici bölümüne tıklayalım.

![Geliştirici](public\chrome_o629RQyMbg.png)

Daha sonra Entegrasyona başlaya tıklayarak devam edelim. Menü kısmından SanalPos Çözümleri kısmından hangi türde ödeme istediğinizi belirleyin.

`Eğer 3D kullanmak istiyorsanız 3D Secure ile Ödeme seçeneğini kullanın.`

![sanalpos](public/chrome_bb7CjPIwmM.png)

Dinamik ödemede wirecard kart bilgileri girmek için size otamatik arayüzünü sunuyor. Eğer siz kendi arayüzünüzü oluşturmak isterseniz statik ödeme seçeneğini seçebilirsiniz.

Ben `Dinamik Ortak Ödeme` seçeneği üzerinden devam edeceğim.
(Ödeme işlemlerinin adam adım olması taraftarı olduğum için bunu daha mantıklı buluyorum)

### Şimdi Kodlamaya Başlayalım..

Wirecard servisi bizden xml tarzında request ister ve sonucunda response döndürür. Yani bizden kullanıcı adı, ödenecek ücret, işlem başarılı ve başarıszı olduğğu durumlarda gidilmesi gereken sayfa uzantı  bilgilerini xml şeklinde servise request yapmamızı ister.
![request](public/chrome_GaNS9qUj4S.png)
 Bunu sonucunda hata var ise hata mesajını, hata yok ise bize responseUrl gönderir.
 ![response](public/chrome_iaJysVeRul.png)
  Bu gelen url ile sitenin sunduğu ödeme sayfasına yönlendir. Bu kısımdan sonra gerçekleşecek işlemler wirekard ile kullanıcı arasındadır. 

  Bir çok yazılım dilleri ile projenize entegre edebilirsiniz. Ben php de nasıl yapıldığını anlatacam.

`Bir sonraki adımı daha iyi anlamanız için wirecard'ın sitesindeki değişkenleri incelemenizi tavsiye ederim.`

````php
//BaseModel.php
class Token
{
    public $UserCode; 
    public $Pin;
    public $BaseUrl;
}
class Product
{
    public $ProductId; 
    public $ProductCategory; 
    public $ProductDescription; 
    public $Price; 
    public $Unit;    
}
class Input
{
    public $Content; 
    public $Gsm;
    public $RequestGsmOperator; 
    public $RequestGsmType; 
    public $MPAY; 
    public $SendOrderResult; 
    public $PaymentTypeId; 
    public $ReceivedSMSObjectId; 
    public $ProductList;
    public $SendNotificationSMS; 
    public $OnSuccessfulSMS;
    public $OnErrorSMS; 
    public $Url; 
    public $SuccessfulPageUrl; 
    public $ErrorPageUrl; 
    public $Country; 
    public $Currency; 
    public $Extra;
    public $TurkcellServiceId;
    public $CustomerIpAddress;
    public $BeginDate;
    public $EndDate;
    public $ProductId;
    public $OrderChannelId;
    public $Active;
    public $SubscriberType;
    public $StartDateMin;
    public $StartDateMax;
    public $LastSuccessfulPaymentDateMin;
    public $LastSuccessfulPaymentDateMax;
    public $SubscriberId;
}
class CreditCardInfo
{
    public $CreditCardNo;
    public $OwnerName;
    public $ExpireYear;
    public $ExpireMonth;
    public $Cvv;
    public $Price;
}
````
Devamı gelecek..