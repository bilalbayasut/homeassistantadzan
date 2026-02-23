# Adzan Automation [![made-for-homeassistant](https://img.shields.io/badge/for-homeassistant-blue)](https://www.home-assistant.io/)

![Adzan Automation Cover](https://github.com/bilalbayasut/homeassistantadzan/blob/7f4ca8caa579247e4487ae50ac00ec7209ec4e19/cover_art.jpeg)

[English](#english) | [Bahasa Indonesia](#bahasa-indonesia)

---

<a name="english"></a>
## 🇺🇸 English

**Automatic Adzan with Home Assistant and Google Nest/Home Speakers.**

This repository provides a modern Home Assistant configuration to play the Adzan (call to prayer) automatically based on your local coordinates using the Aladhan.com REST API. This version is updated to resolve "Legacy template deprecation" warnings by migrating to the modern `template:` integration.

### 🛠 Preparation
1. **Coordinates**: Get your `Latitude` and `Longitude`.
2. **Audio Files**: Place your `azansubuh.mp3` and `Adhan Makkah.mp3` in your Local Media folder.

### 🚀 Implementation Steps

#### 1. Update `configuration.yaml`
Ensure your main configuration loads the separate files for sensors, templates, and automations:
```yaml
sensor: !include sensors.yaml
template: !include templates.yaml
automation: !include automations.yaml

```

#### 2. Configure `sensors.yaml` (REST API)

Replace `<LATITUDE>` and `<LONGITUDE>` with your actual coordinates.

```yaml
- platform: rest
  name: "prayer_times"
  unique_id: prayer_times
  json_attributes: [data]
  resource: "[https://api.aladhan.com/v1/timings?latitude=](https://api.aladhan.com/v1/timings?latitude=)<LATITUDE>&longitude=<LONGITUDE>&method=5&school=2"
  value_template: '{{ value_json["data"]["meta"]["method"]["name"].title() }}'
  scan_interval: 3600

- platform: time_date
  display_options: ['time']

```

#### 3. Configure `templates.yaml` (Modern Format)

This file converts the API data into entities used for triggers.

```yaml
- sensor:
    - name: "Fajr"
      unique_id: fajr_prayer_time
      state: '{{ state_attr("sensor.prayer_times", "data").timings["Fajr"] }}'
    - name: "Sunrise"
      unique_id: sunrise_prayer_time
      state: '{{ state_attr("sensor.prayer_times", "data").timings["Sunrise"] }}'
    - name: "Dhuhr"
      unique_id: dhuhr_prayer_time
      state: '{{ state_attr("sensor.prayer_times", "data").timings["Dhuhr"] }}'
    - name: "Asr"
      unique_id: asr_prayer_time
      state: '{{ state_attr("sensor.prayer_times", "data").timings["Asr"] }}'
    - name: "Maghrib"
      unique_id: maghrib_prayer_time
      state: '{{ state_attr("sensor.prayer_times", "data").timings["Maghrib"] }}'
    - name: "Isha"
      unique_id: isha_prayer_time
      state: '{{ state_attr("sensor.prayer_times", "data").timings["Isha"] }}'
    - name: "Midnight"
      unique_id: midnight_prayer_time
      state: '{{ state_attr("sensor.prayer_times", "data").timings["Midnight"] }}'
    - name: "Tahajjud"
      unique_id: tahajjud_prayer_time
      state: '{{ state_attr("sensor.prayer_times", "data").timings["Imsak"] }}'

```

#### 4. Configure `automations.yaml`

Update `media_player.family_room_speaker` to your actual entity name.

```yaml
- id: prayer_times
  alias: Putar Suara Adzan
  trigger:
    - platform: time
      at: sensor.fajr
      id: fajr
    - platform: time
      at: sensor.dhuhr
      id: dhuhr
    - platform: time
      at: sensor.asr
      id: asr
    - platform: time
      at: sensor.maghrib
      id: maghrib
    - platform: time
      at: sensor.isha
      id: isha
  action:
    - service: media_player.volume_set
      data:
        volume_level: 0.5
      target:
        entity_id: media_player.family_room_speaker
    - choose:
        - conditions:
            - condition: trigger
              id: fajr
          sequence:
            - service: media_player.play_media
              target:
                entity_id: media_player.family_room_speaker
              data:
                media_content_id: "media-source://media_source/local/azansubuh.mp3"
                media_content_type: "audio/mpeg"
      default:
        - service: media_player.play_media
          target:
            entity_id: media_player.family_room_speaker
          data:
            media_content_id: "media-source://media_source/local/Adhan Makkah.mp3"
            media_content_type: "audio/mpeg"
  mode: single

```

---

<a name="bahasa-indonesia"></a>

## 🇮🇩 Bahasa Indonesia

**Otomatisasi Adzan menggunakan Home Assistant dan speaker Google Nest/Home.**

Panduan ini menyediakan konfigurasi sensor jadwal sholat menggunakan REST API Aladhan.com dan otomasi untuk memutar suara Adzan secara otomatis. Kode ini telah diperbarui menggunakan format `template:` terbaru untuk menghilangkan peringatan *legacy template deprecation*.

### 🛠 Persiapan

1. **Koordinat**: Siapkan `Latitude` dan `Longitude` lokasi Anda.
2. **File Audio**: Simpan file `azansubuh.mp3` dan `Adhan Makkah.mp3` di folder media lokal Home Assistant Anda (biasanya di `/config/media/`).

### 🚀 Langkah-Langkah Implementasi

#### 1. Update `configuration.yaml`

Pastikan file konfigurasi utama Anda memisahkan integrasi agar lebih rapi:

```yaml
sensor: !include sensors.yaml
template: !include templates.yaml
automation: !include automations.yaml

```

#### 2. Konfigurasi `sensors.yaml` (REST API)

Masukkan koordinat lokasi Anda pada bagian `<LATITUDE>` dan `<LONGITUDE>`.

```yaml
- platform: rest
  name: "prayer_times"
  unique_id: prayer_times
  json_attributes:
    - data
  resource: "[https://api.aladhan.com/v1/timings?latitude=](https://api.aladhan.com/v1/timings?latitude=)<LATITUDE>&longitude=<LONGITUDE>&method=5&school=2"
  value_template: '{{ value_json["data"]["meta"]["method"]["name"].title() }}'
  scan_interval: 3600

- platform: time_date
  display_options:
    - "time"

```

#### 3. Konfigurasi `templates.yaml` (Format Modern)

File ini mengubah data mentah dari API menjadi entitas sensor yang stabil untuk memicu otomasi.

```yaml
- sensor:
    - name: "Fajr"
      unique_id: fajr_prayer_time
      state: '{{ state_attr("sensor.prayer_times", "data").timings["Fajr"] }}'
    - name: "Sunrise"
      unique_id: sunrise_prayer_time
      state: '{{ state_attr("sensor.prayer_times", "data").timings["Sunrise"] }}'
    - name: "Dhuhr"
      unique_id: dhuhr_prayer_time
      state: '{{ state_attr("sensor.prayer_times", "data").timings["Dhuhr"] }}'
    - name: "Asr"
      unique_id: asr_prayer_time
      state: '{{ state_attr("sensor.prayer_times", "data").timings["Asr"] }}'
    - name: "Maghrib"
      unique_id: maghrib_prayer_time
      state: '{{ state_attr("sensor.prayer_times", "data").timings["Maghrib"] }}'
    - name: "Isha"
      unique_id: isha_prayer_time
      state: '{{ state_attr("sensor.prayer_times", "data").timings["Isha"] }}'
    - name: "Midnight"
      unique_id: midnight_prayer_time
      state: '{{ state_attr("sensor.prayer_times", "data").timings["Midnight"] }}'
    - name: "Tahajjud"
      unique_id: tahajjud_prayer_time
      state: '{{ state_attr("sensor.prayer_times", "data").timings["Imsak"] }}'

```

#### 4. Konfigurasi `automations.yaml`

Ganti `media_player.family_room_speaker` dengan nama entitas speaker Google Nest Anda.

```yaml
# Gunakan kode otomasi yang sama seperti di bagian English di atas.

```

## 📝 Credits

* **REST API**: [Aladhan.com](https://aladhan.com/prayer-times-api)
* **Audio Adzan**: [PrayTimes.org](http://praytimes.org/audio/)
* **Referensi**: [Home Assistant Community](https://community.home-assistant.io/t/hanafi-islamic-prayer-time/147440/10)
