# Home Assistant [![made-for-homeassistant](https://img.shields.io/badge/for-homeassistant-blue)](https://www.home-assistant.io/)
***Adzan otomatis dengan homeassistant dan google nest***

Sensor Jadwal Sholat untuk Home Assistant dan Automation integrasi _Google Smart Speaker Media Player_
untuk memainkan lantunan Suara Adzan sesuai jadwal sholat.

**Yang perlu disiapkan :**
- Longitude
- Lattitude

**Yang perlu di lakukan :**
- Membuat sensor jadwal waktu sholat menggunakan REST API dari aladhan.com, menyesuaikan lokasi LATITUDE & LONGITUDE dengan mengedit file sensors.yaml
- Membuat automation dengan trigger sensor waktu sholat, dengan action media_player.play_media memutar file mp3. edit file automation.yaml
- ganti `change_this_into_your_google_nest_name` ke nama media / google nest anda
```yaml
target:
        entity_id: media_player.change_this_into_your_google_nest_name
```

**Credits :**
- Ref : https://community.home-assistant.io/t/hanafi-islamic-prayer-time/147440/10
- REST API : https://aladhan.com/prayer-times-api
- Mp3 Suara Adzan : http://praytimes.org/audio/