# API BMKG
https://api.bmkg.go.id/publik/prakiraan-cuaca?adm4=32.75.03.1006

## Respon json:
```json
{
    "lokasi": {
        "adm1": "32",
        "adm2": "32.75",
        "adm3": "32.75.03",
        "adm4": "32.75.03.1006",
        "provinsi": "Jawa Barat",
        "kotkab": "Kota Bekasi",
        "kecamatan": "Bekasi Utara",
        "desa": "Harapanjaya",
        "lon": 106.9899360468,
        "lat": -6.2077430613,
        "timezone": "Asia/Jakarta"
    },
    "data": [
        {
            "lokasi": {
                "adm1": "32",
                "adm2": "32.75",
                "adm3": "32.75.03",
                "adm4": "32.75.03.1006",
                "provinsi": "Jawa Barat",
                "kotkab": "Kota Bekasi",
                "kecamatan": "Bekasi Utara",
                "desa": "Harapanjaya",
                "lon": 106.9899360468,
                "lat": -6.2077430613,
                "timezone": "+0700",
                "type": "adm4"
            },
            "cuaca": [
                [
                    {
                        "datetime": "2026-02-28T06:00:00Z",
                        "t": 30,
                        "tcc": 100,
                        "tp": 0,
                        "weather": 3,
                        "weather_desc": "Berawan",
                        "weather_desc_en": "Mostly Cloudy",
                        "wd_deg": 294,
                        "wd": "W",
                        "wd_to": "E",
                        "ws": 12,
                        "hu": 72,
                        "vs": 9998,
                        "vs_text": "< 10 km",
                        "time_index": "5-6",
                        "analysis_date": "2026-02-28T00:00:00",
                        "image": "https://api-apps.bmkg.go.id/storage/icon/cuaca/berawan-am.svg",
                        "utc_datetime": "2026-02-28 06:00:00",
                        "local_datetime": "2026-02-28 13:00:00"
                    },
                    {
                        "datetime": "2026-02-28T09:00:00Z",
                        "t": 27,
                        "tcc": 100,
                        "tp": 0,
                        "weather": 3,
                        "weather_desc": "Berawan",
                        "weather_desc_en": "Mostly Cloudy",
                        "wd_deg": 279,
                        "wd": "W",
                        "wd_to": "E",
                        "ws": 12,
                        "hu": 81,
                        "vs": 10014,
                        "vs_text": "> 10 km",
                        "time_index": "8-9",
                        "analysis_date": "2026-02-28T00:00:00",
                        "image": "https://api-apps.bmkg.go.id/storage/icon/cuaca/berawan-am.svg",
                        "utc_datetime": "2026-02-28 09:00:00",
                        "local_datetime": "2026-02-28 16:00:00"
                    },
                    {
                        "datetime": "2026-02-28T12:00:00Z",
                        "t": 26,
                        "tcc": 100,
                        "tp": 0,
                        "weather": 3,
                        "weather_desc": "Berawan",
                        "weather_desc_en": "Mostly Cloudy",
                        "wd_deg": 243,
                        "wd": "SW",
                        "wd_to": "NE",
                        "ws": 5,
                        "hu": 89,
                        "vs": 9985,
                        "vs_text": "< 10 km",
                        "time_index": "11-12",
                        "analysis_date": "2026-02-28T00:00:00",
                        "image": "https://api-apps.bmkg.go.id/storage/icon/cuaca/berawan-pm.svg",
                        "utc_datetime": "2026-02-28 12:00:00",
                        "local_datetime": "2026-02-28 19:00:00"
                    },
                    {
                        "datetime": "2026-02-28T15:00:00Z",
                        "t": 24,
                        "tcc": 100,
                        "tp": 0,
                        "weather": 3,
                        "weather_desc": "Berawan",
                        "weather_desc_en": "Mostly Cloudy",
                        "wd_deg": 268,
                        "wd": "SW",
                        "wd_to": "NE",
                        "ws": 5.7,
                        "hu": 92,
                        "vs": 10014,
                        "vs_text": "> 10 km",
                        "time_index": "14-15",
                        "analysis_date": "2026-02-28T00:00:00",
                        "image": "https://api-apps.bmkg.go.id/storage/icon/cuaca/berawan-pm.svg",
                        "utc_datetime": "2026-02-28 15:00:00",
                        "local_datetime": "2026-02-28 22:00:00"
                    }
                ],
                [
                    {
                        "datetime": "2026-02-28T18:00:00Z",
                        "t": 23,
                        "tcc": 100,
                        "tp": 0,
                        "weather": 3,
                        "weather_desc": "Berawan",
                        "weather_desc_en": "Mostly Cloudy",
                        "wd_deg": 271,
                        "wd": "W",
                        "wd_to": "E",
                        "ws": 5.1,
                        "hu": 92,
                        "vs": 9999,
                        "vs_text": "< 10 km",
                        "time_index": "17-18",
                        "analysis_date": "2026-02-28T00:00:00",
                        "image": "https://api-apps.bmkg.go.id/storage/icon/cuaca/berawan-pm.svg",
                        "utc_datetime": "2026-02-28 18:00:00",
                        "local_datetime": "2026-03-01 01:00:00"
                    },
                    {
                        "datetime": "2026-02-28T21:00:00Z",
                        "t": 23,
                        "tcc": 100,
                        "tp": 0,
                        "weather": 3,
                        "weather_desc": "Berawan",
                        "weather_desc_en": "Mostly Cloudy",
                        "wd_deg": 258,
                        "wd": "SW",
                        "wd_to": "NE",
                        "ws": 11.4,
                        "hu": 92,
                        "vs": 9987,
                        "vs_text": "< 10 km",
                        "time_index": "20-21",
                        "analysis_date": "2026-02-28T00:00:00",
                        "image": "https://api-apps.bmkg.go.id/storage/icon/cuaca/berawan-pm.svg",
                        "utc_datetime": "2026-02-28 21:00:00",
                        "local_datetime": "2026-03-01 04:00:00"
                    },
                    {
                        "datetime": "2026-03-01T00:00:00Z",
                        "t": 26,
                        "tcc": 99,
                        "tp": 0,
                        "weather": 3,
                        "weather_desc": "Berawan",
                        "weather_desc_en": "Mostly Cloudy",
                        "wd_deg": 247,
                        "wd": "SW",
                        "wd_to": "NE",
                        "ws": 7.7,
                        "hu": 86,
                        "vs": 10009,
                        "vs_text": "> 10 km",
                        "time_index": "23-24",
                        "analysis_date": "2026-02-28T00:00:00",
                        "image": "https://api-apps.bmkg.go.id/storage/icon/cuaca/berawan-am.svg",
                        "utc_datetime": "2026-03-01 00:00:00",
                        "local_datetime": "2026-03-01 07:00:00"
                    },
                    {
                        "datetime": "2026-03-01T03:00:00Z",
                        "t": 30,
                        "tcc": 54,
                        "tp": 0,
                        "weather": 1,
                        "weather_desc": "Cerah",
                        "weather_desc_en": "Sunny",
                        "wd_deg": 276,
                        "wd": "W",
                        "wd_to": "E",
                        "ws": 11.2,
                        "hu": 69,
                        "vs": 10006,
                        "vs_text": "> 10 km",
                        "time_index": "26-27",
                        "analysis_date": "2026-02-28T00:00:00",
                        "image": "https://api-apps.bmkg.go.id/storage/icon/cuaca/cerah-am.svg",
                        "utc_datetime": "2026-03-01 03:00:00",
                        "local_datetime": "2026-03-01 10:00:00"
                    },
                    {
                        "datetime": "2026-03-01T06:00:00Z",
                        "t": 30,
                        "tcc": 80,
                        "tp": 0,
                        "weather": 2,
                        "weather_desc": "Cerah Berawan",
                        "weather_desc_en": "Partly Cloudy",
                        "wd_deg": 316,
                        "wd": "NW",
                        "wd_to": "SE",
                        "ws": 11.9,
                        "hu": 68,
                        "vs": 10012,
                        "vs_text": "> 10 km",
                        "time_index": "29-30",
                        "analysis_date": "2026-02-28T00:00:00",
                        "image": "https://api-apps.bmkg.go.id/storage/icon/cuaca/cerah berawan-am.svg",
                        "utc_datetime": "2026-03-01 06:00:00",
                        "local_datetime": "2026-03-01 13:00:00"
                    },
                    {
                        "datetime": "2026-03-01T09:00:00Z",
                        "t": 28,
                        "tcc": 100,
                        "tp": 0,
                        "weather": 3,
                        "weather_desc": "Berawan",
                        "weather_desc_en": "Mostly Cloudy",
                        "wd_deg": 316,
                        "wd": "NW",
                        "wd_to": "SE",
                        "ws": 9.7,
                        "hu": 76,
                        "vs": 10005,
                        "vs_text": "> 10 km",
                        "time_index": "32-33",
                        "analysis_date": "2026-02-28T00:00:00",
                        "image": "https://api-apps.bmkg.go.id/storage/icon/cuaca/berawan-am.svg",
                        "utc_datetime": "2026-03-01 09:00:00",
                        "local_datetime": "2026-03-01 16:00:00"
                    },
                    {
                        "datetime": "2026-03-01T12:00:00Z",
                        "t": 26,
                        "tcc": 100,
                        "tp": 0,
                        "weather": 3,
                        "weather_desc": "Berawan",
                        "weather_desc_en": "Mostly Cloudy",
                        "wd_deg": 233,
                        "wd": "SW",
                        "wd_to": "NE",
                        "ws": 3.8,
                        "hu": 90,
                        "vs": 9991,
                        "vs_text": "< 10 km",
                        "time_index": "35-36",
                        "analysis_date": "2026-02-28T00:00:00",
                        "image": "https://api-apps.bmkg.go.id/storage/icon/cuaca/berawan-pm.svg",
                        "utc_datetime": "2026-03-01 12:00:00",
                        "local_datetime": "2026-03-01 19:00:00"
                    },
                    {
                        "datetime": "2026-03-01T15:00:00Z",
                        "t": 25,
                        "tcc": 100,
                        "tp": 0,
                        "weather": 3,
                        "weather_desc": "Berawan",
                        "weather_desc_en": "Mostly Cloudy",
                        "wd_deg": 268,
                        "wd": "SW",
                        "wd_to": "NE",
                        "ws": 10.1,
                        "hu": 86,
                        "vs": 9998,
                        "vs_text": "< 10 km",
                        "time_index": "38-39",
                        "analysis_date": "2026-02-28T00:00:00",
                        "image": "https://api-apps.bmkg.go.id/storage/icon/cuaca/berawan-pm.svg",
                        "utc_datetime": "2026-03-01 15:00:00",
                        "local_datetime": "2026-03-01 22:00:00"
                    }
                ],
                [
                    {
                        "datetime": "2026-03-01T18:00:00Z",
                        "t": 22,
                        "tcc": 100,
                        "tp": 0,
                        "weather": 3,
                        "weather_desc": "Berawan",
                        "weather_desc_en": "Mostly Cloudy",
                        "wd_deg": 261,
                        "wd": "SW",
                        "wd_to": "NE",
                        "ws": 9.2,
                        "hu": 88,
                        "vs": 9991,
                        "vs_text": "< 10 km",
                        "time_index": "41-42",
                        "analysis_date": "2026-02-28T00:00:00",
                        "image": "https://api-apps.bmkg.go.id/storage/icon/cuaca/berawan-pm.svg",
                        "utc_datetime": "2026-03-01 18:00:00",
                        "local_datetime": "2026-03-02 01:00:00"
                    },
                    {
                        "datetime": "2026-03-01T21:00:00Z",
                        "t": 23,
                        "tcc": 100,
                        "tp": 0,
                        "weather": 3,
                        "weather_desc": "Berawan",
                        "weather_desc_en": "Mostly Cloudy",
                        "wd_deg": 253,
                        "wd": "SW",
                        "wd_to": "NE",
                        "ws": 11,
                        "hu": 87,
                        "vs": 10003,
                        "vs_text": "> 10 km",
                        "time_index": "44-45",
                        "analysis_date": "2026-02-28T00:00:00",
                        "image": "https://api-apps.bmkg.go.id/storage/icon/cuaca/berawan-pm.svg",
                        "utc_datetime": "2026-03-01 21:00:00",
                        "local_datetime": "2026-03-02 04:00:00"
                    },
                    {
                        "datetime": "2026-03-02T00:00:00Z",
                        "t": 27,
                        "tcc": 100,
                        "tp": 0,
                        "weather": 3,
                        "weather_desc": "Berawan",
                        "weather_desc_en": "Mostly Cloudy",
                        "wd_deg": 268,
                        "wd": "SW",
                        "wd_to": "NE",
                        "ws": 12.5,
                        "hu": 81,
                        "vs": 10000,
                        "vs_text": "< 11 km",
                        "time_index": "47-48",
                        "analysis_date": "2026-02-28T00:00:00",
                        "image": "https://api-apps.bmkg.go.id/storage/icon/cuaca/berawan-am.svg",
                        "utc_datetime": "2026-03-02 00:00:00",
                        "local_datetime": "2026-03-02 07:00:00"
                    },
                    {
                        "datetime": "2026-03-02T03:00:00Z",
                        "t": 30,
                        "tcc": 98,
                        "tp": 0,
                        "weather": 3,
                        "weather_desc": "Berawan",
                        "weather_desc_en": "Mostly Cloudy",
                        "wd_deg": 298,
                        "wd": "W",
                        "wd_to": "E",
                        "ws": 16.1,
                        "hu": 71,
                        "vs": 8576,
                        "vs_text": "< 9 km",
                        "time_index": "50-51",
                        "analysis_date": "2026-02-28T00:00:00",
                        "image": "https://api-apps.bmkg.go.id/storage/icon/cuaca/berawan-am.svg",
                        "utc_datetime": "2026-03-02 03:00:00",
                        "local_datetime": "2026-03-02 10:00:00"
                    },
                    {
                        "datetime": "2026-03-02T06:00:00Z",
                        "t": 29,
                        "tcc": 74,
                        "tp": 0,
                        "weather": 2,
                        "weather_desc": "Cerah Berawan",
                        "weather_desc_en": "Partly Cloudy",
                        "wd_deg": 298,
                        "wd": "W",
                        "wd_to": "E",
                        "ws": 16.1,
                        "hu": 77,
                        "vs": 8576,
                        "vs_text": "< 9 km",
                        "time_index": "53-54",
                        "analysis_date": "2026-02-28T00:00:00",
                        "image": "https://api-apps.bmkg.go.id/storage/icon/cuaca/cerah berawan-am.svg",
                        "utc_datetime": "2026-03-02 06:00:00",
                        "local_datetime": "2026-03-02 13:00:00"
                    },
                    {
                        "datetime": "2026-03-02T09:00:00Z",
                        "t": 28,
                        "tcc": 100,
                        "tp": 0,
                        "weather": 3,
                        "weather_desc": "Berawan",
                        "weather_desc_en": "Mostly Cloudy",
                        "wd_deg": 283,
                        "wd": "W",
                        "wd_to": "E",
                        "ws": 1.9,
                        "hu": 79,
                        "vs": 9990,
                        "vs_text": "< 10 km",
                        "time_index": "56-57",
                        "analysis_date": "2026-02-28T00:00:00",
                        "image": "https://api-apps.bmkg.go.id/storage/icon/cuaca/berawan-am.svg",
                        "utc_datetime": "2026-03-02 09:00:00",
                        "local_datetime": "2026-03-02 16:00:00"
                    },
                    {
                        "datetime": "2026-03-02T12:00:00Z",
                        "t": 26,
                        "tcc": 100,
                        "tp": 0,
                        "weather": 3,
                        "weather_desc": "Berawan",
                        "weather_desc_en": "Mostly Cloudy",
                        "wd_deg": 283,
                        "wd": "W",
                        "wd_to": "E",
                        "ws": 1.9,
                        "hu": 88,
                        "vs": 9990,
                        "vs_text": "< 10 km",
                        "time_index": "59-60",
                        "analysis_date": "2026-02-28T00:00:00",
                        "image": "https://api-apps.bmkg.go.id/storage/icon/cuaca/berawan-pm.svg",
                        "utc_datetime": "2026-03-02 12:00:00",
                        "local_datetime": "2026-03-02 19:00:00"
                    },
                    {
                        "datetime": "2026-03-02T15:00:00Z",
                        "t": 24,
                        "tcc": 100,
                        "tp": 0,
                        "weather": 3,
                        "weather_desc": "Berawan",
                        "weather_desc_en": "Mostly Cloudy",
                        "wd_deg": 259,
                        "wd": "SW",
                        "wd_to": "NE",
                        "ws": 10.4,
                        "hu": 89,
                        "vs": 28899,
                        "vs_text": "> 10 km",
                        "time_index": "62-63",
                        "analysis_date": "2026-02-28T00:00:00",
                        "image": "https://api-apps.bmkg.go.id/storage/icon/cuaca/berawan-pm.svg",
                        "utc_datetime": "2026-03-02 15:00:00",
                        "local_datetime": "2026-03-02 22:00:00"
                    }
                ]
            ]
        }
    ]
}
```

## list value parameter key cuaca yang digunakan dari api BMKG

|weather|weather_desc (ID)|weather_desc_en|
|---|---|---|
|0|Cerah|Clear|
|1|Cerah|Sunny|
|2|Cerah Berawan|Partly Cloudy|
|3|Berawan|Mostly Cloudy|
|4|Berawan Tebal|Overcast|
|5|Udara Kabur|Haze|
|10|Asap|Smoke|
|45|Kabut|Fog|
|60|Hujan Ringan|Light Rain|
|61|Hujan Sedang|Moderate Rain|
|63|Hujan Lebat|Heavy Rain|
|80|Hujan Lokal|Local Rain|
|95|Hujan Petir|Thunderstorm|
|97|Hujan Petir Lebat|Severe Thunderstorm|

# API Waktu Shalat
https://api.aladhan.com/v1/timingsByCity/28-02-2026?city=Bekasi&country=ID&state=Jawa+Barat&method=20&shafaq=general&tune=4%2C2%2C3%2C3%2C3%2C3%2C-6%2C2%2C-6&school=0&timezonestring=Asia%2FJakarta&calendarMethod=UAQ

## Respon json:
```json
{
    "code": 200,
    "status": "OK",
    "data": {
        "timings": {
            "Fajr": "04:42",
            "Sunrise": "06:01",
            "Dhuhr": "12:08",
            "Asr": "15:11",
            "Sunset": "18:05",
            "Maghrib": "18:14",
            "Isha": "19:23",
            "Imsak": "04:34",
            "Midnight": "23:59",
            "Firstthird": "22:07",
            "Lastthird": "02:02"
        },
        "date": {
            "readable": "28 Feb 2026",
            "timestamp": "1772236800",
            "hijri": {
                "date": "11-09-1447",
                "format": "DD-MM-YYYY",
                "day": "11",
                "weekday": {
                    "en": "Al Sabt",
                    "ar": "السبت"
                },
                "month": {
                    "number": 9,
                    "en": "Ramaḍān",
                    "ar": "رَمَضان",
                    "days": 30
                },
                "year": "1447",
                "designation": {
                    "abbreviated": "AH",
                    "expanded": "Anno Hegirae"
                },
                "holidays": [],
                "adjustedHolidays": [],
                "method": "UAQ"
            },
            "gregorian": {
                "date": "28-02-2026",
                "format": "DD-MM-YYYY",
                "day": "28",
                "weekday": {
                    "en": "Saturday"
                },
                "month": {
                    "number": 2,
                    "en": "February"
                },
                "year": "2026",
                "designation": {
                    "abbreviated": "AD",
                    "expanded": "Anno Domini"
                },
                "lunarSighting": false
            }
        },
        "meta": {
            "latitude": 8.8888888,
            "longitude": 7.7777777,
            "timezone": "Asia/Jakarta",
            "method": {
                "id": 20,
                "name": "Kementerian Agama Republik Indonesia",
                "params": {
                    "Fajr": 20,
                    "Isha": 18
                },
                "location": {
                    "latitude": -6.2087634,
                    "longitude": 106.845599
                }
            },
            "latitudeAdjustmentMethod": "ANGLE_BASED",
            "midnightMode": "STANDARD",
            "school": "STANDARD",
            "offset": {
                "Imsak": "4",
                "Fajr": "2",
                "Sunrise": "3",
                "Dhuhr": "3",
                "Asr": "3",
                "Maghrib": "3",
                "Sunset": "-6",
                "Isha": "2",
                "Midnight": "-6"
            }
        }
    }
}
```

