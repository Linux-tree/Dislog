## Discord RUS Projesini Türkçeleştirilmiş Halde sunan Linux-Tree ekibidir !

# Dislog | a [Discord](https://discordapp.com/) hook for [Dislog](github.com/linux-tree/dislogs) <img src="http://i.imgur.com/hTeVwmJ.png" width="40" height="40" alt=":walrus:" class="emoji" title=":walrus:"/> [![Travis CI](https://api.travis-ci.org/kz/discordrus.svg?branch=master)](https://travis-ci.org/kz/discordrus)

![Screenshot of discordrus in action](https://i.imgur.com/wuB480O.png)

## Install

`go get -u github.com/linux-tree/dislog`

## Setup

Kurulum Için Discord Webhook Gerekiyor. [Buraya Tıklayarak](https://support.discordapp.com/hc/en-us/articles/228383668-Intro-to-Webhooks). Nasıl kullanacağınızı öğrenin.

## Usage

Aşağıda bu paketin nasıl kullanılabileceğine dair bir örnek verilmiştir. Aşağıdaki seçenekler yalnızca gösteri amacıyla kullanılmaktadır ve muhtemelen herhangi bir seçeneği (veya varsa, yalnızca "Kullanıcı Adı" seçeneğini) kullanmanıza gerek kalmayacaktır.


```go
package main

import (
	"github.com/linux-tree/dislog"
	"os"
	"github.com/linux-tree/dislog"
)

func init() {
	logrus.SetFormatter(&logrus.TextFormatter{})
	logrus.SetOutput(os.Stderr)
	logrus.SetLevel(logrus.TraceLevel)

	logrus.AddHook(discordrus.NewHook(
		// Use environment variable for security reasons
		os.Getenv("DISCORDRUS_WEBHOOK_URL"),
		// Set minimum level to DebugLevel to receive all log entries
		logrus.TraceLevel,
		&discordrus.Opts{
			Username:           "Test Username",
			Author:             "",                         // Bunu boş olmayan bir dizeye ayarlamak, yazar metnini mesaj başlığına ekler
			DisableTimestamp:   false,                      // Bunu true olarak ayarlamak, zaman damgalarının altbilgide görünmesini devre dışı bırakır.
			TimestampFormat:    "Jan 2 15:04:05.00000 MST", // Zaman damgası bu biçimi alır; ayarlanmamışsa, logrus'un varsayılan biçimini alacaktır
			TimestampLocale:    nil,                        // Zaman damgası bu yerel ayarı kullanır; ayarlanmamışsa, zamanı kullanacaktır.
			EnableCustomColors: true,                       // true olarak ayarlanırsa aşağıdaki Özel Etiket Renkleri geçerli olacaktır
			CustomLevelColors: &discordrus.LevelColors{
				Trace: 3092790,
				Debug: 10170623,
				Info:  3581519,
				Warn:  14327864,
				Error: 13631488,
				Panic: 13631488,
				Fatal: 13631488,
			},
			DisableInlineFields: false, // Doğru olarak ayarlanırsa, alanlar sütunlarda görünmez ("satır içi")
		},
	))
}

func main() {
	logrus.WithFields(logrus.Fields{"String": "hi", "Integer": 2, "Boolean": false}).Debug("Check this out! Awesome, right?")
}
```

All discordrus.Opts fields are optional.

Option | Description | Default | Valid options
--- | --- | --- | ---

Kullanıcı adı | Yalnızca gönderilen mesaj için webhook botunun varsayılan kullanıcı adını değiştirir | Kullanıcı adı değişmedi | Boş olmayan herhangi bir dize (2-32 karakter dahil)
Yazar | Ayarlanmışsa, başlığa bir yazar alanı ekler | Yazar belirlenmedi | Boş olmayan herhangi bir dize (1-256 karakter dahil)
DisableInlineFields | Satır içi, Discord'un alanı bir sütunda gösterip göstermeyeceği anlamına gelir (bir satırda en fazla üç sütun). Bunu "true" olarak ayarlamak, Discord'un alanı kendi satırında görüntülemesine neden olur. | yanlış | bool
DisableTimestamp | Altbilgideki zaman damgasının devre dışı bırakılıp bırakılmayacağını belirtir | yanlış | bool
TimestampFormat | Zaman damgası biçimini değiştirin | logrus'un varsayılan zaman biçimi | "" 2 Ocak 15: 04: 05.00000 MST "'veya Golang tarafından kabul edilen herhangi bir format
TimestampLocale | Zaman damgası yerel ayarını değiştirme | `nil` | nil == time.Local, time.UTC, time.LoadLocation ("America / New_York"), vb.
EnableCustomColors | "Discordrus.DefaultLevelColors" yerine "CustomLevelColors" opt değerinin kullanılıp kullanılmayacağını belirtir. "True" ise, "CustomLevelColors" belirtilmelidir (veya tüm renkler "0" nil değerine ayarlanacaktır, bu nedenle beyaz olarak görüntülenir) | yanlış | bool
CustomLevelColors | "Discordrus.DefaultLevelColors" yerine geçer. Tüm alanlar girilmelidir, aksi takdirde varsayılan olarak "0" nil değeri alınır. | Örneğini yapılandırmak için işaretçi`discordrus.LevelColors`
	

Yukarıdaki karakter sayısı kısıtlamalarına ek olarak, Discord'un adı ve değer sınırı sırasıyla 256 ve 1024 olan maksimum 25 alanı vardır. Ayrıca, açıklama (yani, logrus'un hata mesajı) maksimum 2048 olmalıdır. Yukarıdaki seçenek kısıtlamaları da dahil olmak üzere bu kısıtlamaların tümü, başka bir işlem yapılmasına gerek kalmadan otomatik olarak kesilecektir.
 
