# Лаборатори №3: Чанарын сценарио → SLO → k6 threshold

Оюутан: М. Батдорж
Код: B252270136

## Гурван чанарын сценарио

### 1. Performance сценарио

| Хэсэг | Агуулга |
|---|---|
| Тойм | `/cart/add` endpoint хэвийн ачаалалд хурдан хариу өгөх ёстой |
| Системийн төлөв | API идэвхтэй, гадаад dependency алга |
| Орчны төлөв | 20 VU тогтмол ачаалал, 1 минутын турш |
| Гадаад өдөөлт | Хэрэглэгч сагсанд бараа нэмэх хүсэлт илгээнэ |
| Шаардлагатай хариу | HTTP 200, `{ok:true, items:1}` |
| Хэмжүүр | p95 хариу хугацаа < 200мс |

### 2. Reliability сценарио

| Хэсэг | Агуулга |
|---|---|
| Тойм | `/pay` endpoint нь төлбөрийн үйлдлийг найдвартай гүйцэтгэх ёстой |
| Системийн төлөв | API идэвхтэй, gateway горим санамсаргүй алдаа гаргадаг |
| Орчны төлөв | 20 VU тогтмол ачаалал, 1 минутын турш |
| Гадаад өдөөлт | Хэрэглэгч төлбөр төлөх хүсэлт илгээнэ |
| Шаардлагатай хариу | HTTP 200, `{paid:true}` — эсвэл алдааны хувь тогтоосон хэмжээнээс бага байх |
| Хэмжүүр | Error rate (POFOD) < 8% |

### 3. Availability сценарио

| Хэсэг | Агуулга |
|---|---|
| Тойм | Сервер гэнэт унасан ч систем богино хугацаанд сэргэх ёстой |
| Системийн төлөв | Сервер ажиллаж байгаад санамсаргүйгээр унана, дараа нь дахин асна |
| Орчны төлөв | 10 VU, 2 минутын турш, дунд нь 10 секунд зогсолттой |
| Гадаад өдөөлт | Сервер эвдрэл (crash) — процесс зогсоод дахин асна |
| Шаардлагатай хариу | Зогсолтын дараа систем автоматаар сэргэж хүсэлт хүлээж авах |
| Хэмжүүр | Availability ≥ 90%, сэргэх хугацаа ≤ 10с |

## SLO хүснэгт

| Сценарио | SLI (юуг хэмжих) | Босго | Цонх/нөхцөл |
|---|---|---|---|
| Performance | `/cart/add` p95 latency | < 200мс | 20 VU тогтмол ачаалалд, 1 мин |
| Reliability | `/pay` error rate | < 8% | 1 мин, 20 VU |
| Availability | Бүх хүсэлтийн амжилтын хувь | ≥ 90% | 2 мин, 10с зогсолттой |

**Босго сонгосон үндэслэл:** Серверт зориудаар ~5% алдаа суулгасан (`/pay` endpoint) тул 8%-ийн босго нь бодит хэмжигдэхүүнд тулгуурласан, хэт өөдрөг биш, хэт сул ч биш.

**Error budget (цагаар):** 2 минутын цонхонд 90% availability гэдэг нь 10% = 12 секундийн "эвдрэх эрх" гэсэн үг.
{
  echo ""
  echo "## k6 version"
  echo '```'
  k6 version
  echo '```'
  echo ""
  echo "## k6 үр дүн — PASS (results/pass.txt)"
  echo '```'
  cat results/pass.txt
  echo '```'
  echo ""
  echo "## k6 үр дүн — Chaos тест (results/chaos.txt)"
  echo '```'
  cat results/chaos.txt
  echo '```'
  echo ""
  echo "## k6 үр дүн — FAIL (results/fail.txt)"
  echo '```'
  cat results/fail.txt
  echo '```'
} >> README.md


## k6 version
```
k6 v2.3.0+dirty (commit/e088784614-dirty, go1.27.1, linux/amd64)
```

## k6 үр дүн — PASS (results/pass.txt)
```

         /\      Grafana   /‾‾/  
    /\  /  \     |\  __   /  /   
   /  \/    \    | |/ /  /   ‾‾\ 
  /          \   |   (  |  (‾)  |
 / __________ \  |_|\_\  \_____/ 


     execution: local
        script: slo-test.js
        output: -

     scenarios: (100.00%) 1 scenario, 20 max VUs, 1m30s max duration (incl. graceful stop):
              * default: 20 looping VUs for 1m0s (gracefulStop: 30s)


running (0m01.0s), 20/20 VUs, 0 complete and 0 interrupted iterations
default   [   2% ] 20 VUs  0m01.0s/1m0s

running (0m02.0s), 20/20 VUs, 20 complete and 0 interrupted iterations
default   [   3% ] 20 VUs  0m02.0s/1m0s

running (0m03.0s), 20/20 VUs, 40 complete and 0 interrupted iterations
default   [   5% ] 20 VUs  0m03.0s/1m0s

running (0m04.0s), 20/20 VUs, 55 complete and 0 interrupted iterations
default   [   7% ] 20 VUs  0m04.0s/1m0s

running (0m05.0s), 20/20 VUs, 60 complete and 0 interrupted iterations
default   [   8% ] 20 VUs  0m05.0s/1m0s

running (0m06.0s), 20/20 VUs, 80 complete and 0 interrupted iterations
default   [  10% ] 20 VUs  0m06.0s/1m0s

running (0m07.0s), 20/20 VUs, 100 complete and 0 interrupted iterations
default   [  12% ] 20 VUs  0m07.0s/1m0s

running (0m08.0s), 20/20 VUs, 116 complete and 0 interrupted iterations
default   [  13% ] 20 VUs  0m08.0s/1m0s

running (0m09.0s), 20/20 VUs, 121 complete and 0 interrupted iterations
default   [  15% ] 20 VUs  0m09.0s/1m0s

running (0m10.0s), 20/20 VUs, 140 complete and 0 interrupted iterations
default   [  17% ] 20 VUs  0m10.0s/1m0s

running (0m11.0s), 20/20 VUs, 160 complete and 0 interrupted iterations
default   [  18% ] 20 VUs  0m11.0s/1m0s

running (0m12.0s), 20/20 VUs, 177 complete and 0 interrupted iterations
default   [  20% ] 20 VUs  0m12.0s/1m0s

running (0m13.0s), 20/20 VUs, 184 complete and 0 interrupted iterations
default   [  22% ] 20 VUs  0m13.0s/1m0s

running (0m14.0s), 20/20 VUs, 200 complete and 0 interrupted iterations
default   [  23% ] 20 VUs  0m14.0s/1m0s

running (0m15.0s), 20/20 VUs, 220 complete and 0 interrupted iterations
default   [  25% ] 20 VUs  0m15.0s/1m0s

running (0m16.0s), 20/20 VUs, 238 complete and 0 interrupted iterations
default   [  27% ] 20 VUs  0m16.0s/1m0s

running (0m17.0s), 20/20 VUs, 249 complete and 0 interrupted iterations
default   [  28% ] 20 VUs  0m17.0s/1m0s

running (0m18.0s), 20/20 VUs, 260 complete and 0 interrupted iterations
default   [  30% ] 20 VUs  0m18.0s/1m0s

running (0m19.0s), 20/20 VUs, 280 complete and 0 interrupted iterations
default   [  32% ] 20 VUs  0m19.0s/1m0s

running (0m20.0s), 20/20 VUs, 299 complete and 0 interrupted iterations
default   [  33% ] 20 VUs  0m20.0s/1m0s

running (0m21.0s), 20/20 VUs, 311 complete and 0 interrupted iterations
default   [  35% ] 20 VUs  0m21.0s/1m0s

running (0m22.0s), 20/20 VUs, 320 complete and 0 interrupted iterations
default   [  37% ] 20 VUs  0m22.0s/1m0s

running (0m23.0s), 20/20 VUs, 340 complete and 0 interrupted iterations
default   [  38% ] 20 VUs  0m23.0s/1m0s

running (0m24.0s), 20/20 VUs, 360 complete and 0 interrupted iterations
default   [  40% ] 20 VUs  0m24.0s/1m0s

running (0m25.0s), 20/20 VUs, 370 complete and 0 interrupted iterations
default   [  42% ] 20 VUs  0m25.0s/1m0s

running (0m26.0s), 20/20 VUs, 384 complete and 0 interrupted iterations
default   [  43% ] 20 VUs  0m26.0s/1m0s

running (0m27.0s), 20/20 VUs, 400 complete and 0 interrupted iterations
default   [  45% ] 20 VUs  0m27.0s/1m0s

running (0m28.0s), 20/20 VUs, 420 complete and 0 interrupted iterations
default   [  47% ] 20 VUs  0m28.0s/1m0s

running (0m29.0s), 20/20 VUs, 435 complete and 0 interrupted iterations
default   [  48% ] 20 VUs  0m29.0s/1m0s

running (0m30.0s), 20/20 VUs, 444 complete and 0 interrupted iterations
default   [  50% ] 20 VUs  0m30.0s/1m0s

running (0m31.0s), 20/20 VUs, 461 complete and 0 interrupted iterations
default   [  52% ] 20 VUs  0m31.0s/1m0s

running (0m32.0s), 20/20 VUs, 480 complete and 0 interrupted iterations
default   [  53% ] 20 VUs  0m32.0s/1m0s

running (0m33.0s), 20/20 VUs, 496 complete and 0 interrupted iterations
default   [  55% ] 20 VUs  0m33.0s/1m0s

running (0m34.0s), 20/20 VUs, 507 complete and 0 interrupted iterations
default   [  57% ] 20 VUs  0m34.0s/1m0s

running (0m35.0s), 20/20 VUs, 521 complete and 0 interrupted iterations
default   [  58% ] 20 VUs  0m35.0s/1m0s

running (0m36.0s), 20/20 VUs, 540 complete and 0 interrupted iterations
default   [  60% ] 20 VUs  0m36.0s/1m0s

running (0m37.0s), 20/20 VUs, 558 complete and 0 interrupted iterations
default   [  62% ] 20 VUs  0m37.0s/1m0s

running (0m38.0s), 20/20 VUs, 569 complete and 0 interrupted iterations
default   [  63% ] 20 VUs  0m38.0s/1m0s

running (0m39.0s), 20/20 VUs, 583 complete and 0 interrupted iterations
default   [  65% ] 20 VUs  0m39.0s/1m0s

running (0m40.0s), 20/20 VUs, 602 complete and 0 interrupted iterations
default   [  67% ] 20 VUs  0m40.0s/1m0s

running (0m41.0s), 20/20 VUs, 619 complete and 0 interrupted iterations
default   [  68% ] 20 VUs  0m41.0s/1m0s

running (0m42.0s), 20/20 VUs, 634 complete and 0 interrupted iterations
default   [  70% ] 20 VUs  0m42.0s/1m0s

running (0m43.0s), 20/20 VUs, 644 complete and 0 interrupted iterations
default   [  72% ] 20 VUs  0m43.0s/1m0s

running (0m44.0s), 20/20 VUs, 662 complete and 0 interrupted iterations
default   [  73% ] 20 VUs  0m44.0s/1m0s

running (0m45.0s), 20/20 VUs, 680 complete and 0 interrupted iterations
default   [  75% ] 20 VUs  0m45.0s/1m0s

running (0m46.0s), 20/20 VUs, 694 complete and 0 interrupted iterations
default   [  77% ] 20 VUs  0m46.0s/1m0s

running (0m47.0s), 20/20 VUs, 706 complete and 0 interrupted iterations
default   [  78% ] 20 VUs  0m47.0s/1m0s

running (0m48.0s), 20/20 VUs, 722 complete and 0 interrupted iterations
default   [  80% ] 20 VUs  0m48.0s/1m0s

running (0m49.0s), 20/20 VUs, 741 complete and 0 interrupted iterations
default   [  82% ] 20 VUs  0m49.0s/1m0s

running (0m50.0s), 20/20 VUs, 756 complete and 0 interrupted iterations
default   [  83% ] 20 VUs  0m50.0s/1m0s

running (0m51.0s), 20/20 VUs, 769 complete and 0 interrupted iterations
default   [  85% ] 20 VUs  0m51.0s/1m0s

running (0m52.0s), 20/20 VUs, 784 complete and 0 interrupted iterations
default   [  87% ] 20 VUs  0m52.0s/1m0s

running (0m53.0s), 20/20 VUs, 802 complete and 0 interrupted iterations
default   [  88% ] 20 VUs  0m53.0s/1m0s

running (0m54.0s), 20/20 VUs, 817 complete and 0 interrupted iterations
default   [  90% ] 20 VUs  0m54.0s/1m0s

running (0m55.0s), 20/20 VUs, 832 complete and 0 interrupted iterations
default   [  92% ] 20 VUs  0m55.0s/1m0s

running (0m56.0s), 20/20 VUs, 845 complete and 0 interrupted iterations
default   [  93% ] 20 VUs  0m56.0s/1m0s

running (0m57.0s), 20/20 VUs, 862 complete and 0 interrupted iterations
default   [  95% ] 20 VUs  0m57.0s/1m0s

running (0m58.0s), 20/20 VUs, 877 complete and 0 interrupted iterations
default   [  97% ] 20 VUs  0m58.0s/1m0s

running (0m59.0s), 20/20 VUs, 892 complete and 0 interrupted iterations
default   [  98% ] 20 VUs  0m59.0s/1m0s

running (1m00.0s), 20/20 VUs, 905 complete and 0 interrupted iterations
default   [ 100% ] 20 VUs  1m00.0s/1m0s

running (1m01.0s), 04/20 VUs, 922 complete and 0 interrupted iterations
default ↓ [ 100% ] 20 VUs  1m0s


  █ THRESHOLDS 

    checks
    ✓ 'rate>0.90' rate=98.05%

    http_req_duration{name:cart}
    ✓ 'p(95)<200' p(95)=2.93ms

    http_req_duration{name:report}
    ✓ 'p(95)<450' p(95)=394.03ms

    http_req_failed{name:pay}
    ✓ 'rate<0.08' rate=5.83%


  █ TOTAL RESULTS 

    checks_total.......: 2778   45.291177/s
    checks_succeeded...: 98.05% 2724 out of 2778
    checks_failed......: 1.94%  54 out of 2778

    ✓ cart 200
    ✓ report 200
    ✗ pay 200
      ↳  94% — ✓ 872 / ✗ 54

    HTTP
    http_req_duration..............: avg=101.98ms min=153.71µs med=1.85ms   max=401.93ms p(90)=343.36ms p(95)=372.58ms
      { expected_response:true }...: avg=103.98ms min=153.71µs med=1.87ms   max=401.93ms p(90)=343.97ms p(95)=373.08ms
      { name:cart }................: avg=1.86ms   min=498.29µs med=1.74ms   max=11.15ms  p(90)=2.48ms   p(95)=2.93ms  
      { name:report }..............: avg=302.77ms min=201.32ms med=306.03ms max=401.93ms p(90)=382.39ms p(95)=394.03ms
    http_req_failed................: 1.94%  54 out of 2778
      { name:pay }.................: 5.83%  54 out of 926
    http_reqs......................: 2778   45.291177/s

    EXECUTION
    iteration_duration.............: avg=1.3s     min=1.2s     med=1.31s    max=1.41s    p(90)=1.38s    p(95)=1.39s   
    iterations.....................: 926    15.097059/s
    vus............................: 4      min=4          max=20
    vus_max........................: 20     min=20         max=20

    NETWORK
    data_received..................: 697 kB 11 kB/s
    data_sent......................: 247 kB 4.0 kB/s




running (1m01.3s), 00/20 VUs, 926 complete and 0 interrupted iterations
default ✓ [ 100% ] 20 VUs  1m0s
```

## k6 үр дүн — Chaos тест (results/chaos.txt)
```

         /\      Grafana   /‾‾/  
    /\  /  \     |\  __   /  /   
   /  \/    \    | |/ /  /   ‾‾\ 
  /          \   |   (  |  (‾)  |
 / __________ \  |_|\_\  \_____/ 


     execution: local
        script: slo-test.js
        output: -

     scenarios: (100.00%) 1 scenario, 20 max VUs, 2m30s max duration (incl. graceful stop):
              * default: 20 looping VUs for 2m0s (gracefulStop: 30s)


running (0m01.0s), 20/20 VUs, 0 complete and 0 interrupted iterations
default   [   1% ] 20 VUs  0m01.0s/2m0s

running (0m02.0s), 20/20 VUs, 20 complete and 0 interrupted iterations
default   [   2% ] 20 VUs  0m02.0s/2m0s

running (0m03.0s), 20/20 VUs, 40 complete and 0 interrupted iterations
default   [   2% ] 20 VUs  0m03.0s/2m0s

running (0m04.0s), 20/20 VUs, 58 complete and 0 interrupted iterations
default   [   3% ] 20 VUs  0m04.0s/2m0s

running (0m05.0s), 20/20 VUs, 60 complete and 0 interrupted iterations
default   [   4% ] 20 VUs  0m05.0s/2m0s

running (0m06.0s), 20/20 VUs, 80 complete and 0 interrupted iterations
default   [   5% ] 20 VUs  0m06.0s/2m0s

running (0m07.0s), 20/20 VUs, 100 complete and 0 interrupted iterations
default   [   6% ] 20 VUs  0m07.0s/2m0s

running (0m08.0s), 20/20 VUs, 118 complete and 0 interrupted iterations
default   [   7% ] 20 VUs  0m08.0s/2m0s

running (0m09.0s), 20/20 VUs, 121 complete and 0 interrupted iterations
default   [   7% ] 20 VUs  0m09.0s/2m0s

running (0m10.0s), 20/20 VUs, 140 complete and 0 interrupted iterations
default   [   8% ] 20 VUs  0m10.0s/2m0s

running (0m11.0s), 20/20 VUs, 160 complete and 0 interrupted iterations
default   [   9% ] 20 VUs  0m11.0s/2m0s

running (0m12.0s), 20/20 VUs, 179 complete and 0 interrupted iterations
default   [  10% ] 20 VUs  0m12.0s/2m0s

running (0m13.0s), 20/20 VUs, 183 complete and 0 interrupted iterations
default   [  11% ] 20 VUs  0m13.0s/2m0s

running (0m14.0s), 20/20 VUs, 201 complete and 0 interrupted iterations
default   [  12% ] 20 VUs  0m14.0s/2m0s

running (0m15.0s), 20/20 VUs, 220 complete and 0 interrupted iterations
default   [  12% ] 20 VUs  0m15.0s/2m0s

running (0m16.0s), 20/20 VUs, 240 complete and 0 interrupted iterations
default   [  13% ] 20 VUs  0m16.0s/2m0s

running (0m17.0s), 20/20 VUs, 250 complete and 0 interrupted iterations
default   [  14% ] 20 VUs  0m17.0s/2m0s

running (0m18.0s), 20/20 VUs, 261 complete and 0 interrupted iterations
default   [  15% ] 20 VUs  0m18.0s/2m0s

running (0m19.0s), 20/20 VUs, 280 complete and 0 interrupted iterations
default   [  16% ] 20 VUs  0m19.0s/2m0s

running (0m20.0s), 20/20 VUs, 300 complete and 0 interrupted iterations
default   [  17% ] 20 VUs  0m20.0s/2m0s

running (0m21.0s), 20/20 VUs, 317 complete and 0 interrupted iterations
default   [  17% ] 20 VUs  0m21.0s/2m0s

running (0m22.0s), 20/20 VUs, 324 complete and 0 interrupted iterations
default   [  18% ] 20 VUs  0m22.0s/2m0s

running (0m23.0s), 20/20 VUs, 340 complete and 0 interrupted iterations
default   [  19% ] 20 VUs  0m23.0s/2m0s

running (0m24.0s), 20/20 VUs, 360 complete and 0 interrupted iterations
default   [  20% ] 20 VUs  0m24.0s/2m0s

running (0m25.0s), 20/20 VUs, 375 complete and 0 interrupted iterations
default   [  21% ] 20 VUs  0m25.0s/2m0s

running (0m26.0s), 20/20 VUs, 386 complete and 0 interrupted iterations
default   [  22% ] 20 VUs  0m26.0s/2m0s

running (0m27.0s), 20/20 VUs, 401 complete and 0 interrupted iterations
default   [  22% ] 20 VUs  0m27.0s/2m0s

running (0m28.0s), 20/20 VUs, 421 complete and 0 interrupted iterations
default   [  23% ] 20 VUs  0m28.0s/2m0s

running (0m29.0s), 20/20 VUs, 437 complete and 0 interrupted iterations
default   [  24% ] 20 VUs  0m29.0s/2m0s

running (0m30.0s), 20/20 VUs, 449 complete and 0 interrupted iterations
default   [  25% ] 20 VUs  0m30.0s/2m0s

running (0m31.0s), 20/20 VUs, 461 complete and 0 interrupted iterations
default   [  26% ] 20 VUs  0m31.0s/2m0s

running (0m32.0s), 20/20 VUs, 481 complete and 0 interrupted iterations
default   [  27% ] 20 VUs  0m32.0s/2m0s

running (0m33.0s), 20/20 VUs, 500 complete and 0 interrupted iterations
default   [  27% ] 20 VUs  0m33.0s/2m0s

running (0m34.0s), 20/20 VUs, 509 complete and 0 interrupted iterations
default   [  28% ] 20 VUs  0m34.0s/2m0s

running (0m35.0s), 20/20 VUs, 522 complete and 0 interrupted iterations
default   [  29% ] 20 VUs  0m35.0s/2m0s

running (0m36.0s), 20/20 VUs, 540 complete and 0 interrupted iterations
default   [  30% ] 20 VUs  0m36.0s/2m0s

running (0m37.0s), 20/20 VUs, 559 complete and 0 interrupted iterations
default   [  31% ] 20 VUs  0m37.0s/2m0s

running (0m38.0s), 20/20 VUs, 572 complete and 0 interrupted iterations
default   [  32% ] 20 VUs  0m38.0s/2m0s

running (0m39.0s), 20/20 VUs, 584 complete and 0 interrupted iterations
default   [  32% ] 20 VUs  0m39.0s/2m0s

running (0m40.0s), 20/20 VUs, 602 complete and 0 interrupted iterations
default   [  33% ] 20 VUs  0m40.0s/2m0s

running (0m41.0s), 20/20 VUs, 620 complete and 0 interrupted iterations
default   [  34% ] 20 VUs  0m41.0s/2m0s

running (0m42.0s), 20/20 VUs, 637 complete and 0 interrupted iterations
default   [  35% ] 20 VUs  0m42.0s/2m0s

running (0m43.0s), 20/20 VUs, 649 complete and 0 interrupted iterations
default   [  36% ] 20 VUs  0m43.0s/2m0s

running (0m44.0s), 20/20 VUs, 662 complete and 0 interrupted iterations
default   [  37% ] 20 VUs  0m44.0s/2m0s

running (0m45.0s), 20/20 VUs, 681 complete and 0 interrupted iterations
default   [  37% ] 20 VUs  0m45.0s/2m0s

running (0m46.0s), 20/20 VUs, 698 complete and 0 interrupted iterations
default   [  38% ] 20 VUs  0m46.0s/2m0s

running (0m47.0s), 20/20 VUs, 709 complete and 0 interrupted iterations
default   [  39% ] 20 VUs  0m47.0s/2m0s

running (0m48.0s), 20/20 VUs, 723 complete and 0 interrupted iterations
default   [  40% ] 20 VUs  0m48.0s/2m0s

running (0m49.0s), 20/20 VUs, 741 complete and 0 interrupted iterations
default   [  41% ] 20 VUs  0m49.0s/2m0s

running (0m50.0s), 20/20 VUs, 758 complete and 0 interrupted iterations
default   [  42% ] 20 VUs  0m50.0s/2m0s

running (0m51.0s), 20/20 VUs, 773 complete and 0 interrupted iterations
default   [  42% ] 20 VUs  0m51.0s/2m0s

running (0m52.0s), 20/20 VUs, 785 complete and 0 interrupted iterations
default   [  43% ] 20 VUs  0m52.0s/2m0s

running (0m53.0s), 20/20 VUs, 801 complete and 0 interrupted iterations
default   [  44% ] 20 VUs  0m53.0s/2m0s

running (0m54.0s), 20/20 VUs, 819 complete and 0 interrupted iterations
default   [  45% ] 20 VUs  0m54.0s/2m0s

running (0m55.0s), 20/20 VUs, 833 complete and 0 interrupted iterations
default   [  46% ] 20 VUs  0m55.0s/2m0s

running (0m56.0s), 20/20 VUs, 847 complete and 0 interrupted iterations
default   [  47% ] 20 VUs  0m56.0s/2m0s

running (0m57.0s), 20/20 VUs, 863 complete and 0 interrupted iterations
default   [  47% ] 20 VUs  0m57.0s/2m0s

running (0m58.0s), 20/20 VUs, 880 complete and 0 interrupted iterations
default   [  48% ] 20 VUs  0m58.0s/2m0s

running (0m59.0s), 20/20 VUs, 895 complete and 0 interrupted iterations
default   [  49% ] 20 VUs  0m59.0s/2m0s

running (1m00.0s), 20/20 VUs, 909 complete and 0 interrupted iterations
default   [  50% ] 20 VUs  1m00.0s/2m0s

running (1m01.0s), 20/20 VUs, 924 complete and 0 interrupted iterations
default   [  51% ] 20 VUs  1m01.0s/2m0s

running (1m02.0s), 20/20 VUs, 940 complete and 0 interrupted iterations
default   [  52% ] 20 VUs  1m02.0s/2m0s

running (1m03.0s), 20/20 VUs, 957 complete and 0 interrupted iterations
default   [  52% ] 20 VUs  1m03.0s/2m0s

running (1m04.0s), 20/20 VUs, 971 complete and 0 interrupted iterations
default   [  53% ] 20 VUs  1m04.0s/2m0s

running (1m05.0s), 20/20 VUs, 985 complete and 0 interrupted iterations
default   [  54% ] 20 VUs  1m05.0s/2m0s

running (1m06.0s), 20/20 VUs, 1002 complete and 0 interrupted iterations
default   [  55% ] 20 VUs  1m06.0s/2m0s

running (1m07.0s), 20/20 VUs, 1016 complete and 0 interrupted iterations
default   [  56% ] 20 VUs  1m07.0s/2m0s

running (1m08.0s), 20/20 VUs, 1032 complete and 0 interrupted iterations
default   [  57% ] 20 VUs  1m08.0s/2m0s

running (1m09.0s), 20/20 VUs, 1047 complete and 0 interrupted iterations
default   [  57% ] 20 VUs  1m09.0s/2m0s

running (1m10.0s), 20/20 VUs, 1063 complete and 0 interrupted iterations
default   [  58% ] 20 VUs  1m10.0s/2m0s

running (1m11.0s), 20/20 VUs, 1079 complete and 0 interrupted iterations
default   [  59% ] 20 VUs  1m11.0s/2m0s

running (1m12.0s), 20/20 VUs, 1093 complete and 0 interrupted iterations
default   [  60% ] 20 VUs  1m12.0s/2m0s

running (1m13.0s), 20/20 VUs, 1109 complete and 0 interrupted iterations
default   [  61% ] 20 VUs  1m13.0s/2m0s

running (1m14.0s), 20/20 VUs, 1126 complete and 0 interrupted iterations
default   [  62% ] 20 VUs  1m14.0s/2m0s

running (1m15.0s), 20/20 VUs, 1140 complete and 0 interrupted iterations
default   [  62% ] 20 VUs  1m15.0s/2m0s

running (1m16.0s), 20/20 VUs, 1155 complete and 0 interrupted iterations
default   [  63% ] 20 VUs  1m16.0s/2m0s

running (1m17.0s), 20/20 VUs, 1171 complete and 0 interrupted iterations
default   [  64% ] 20 VUs  1m17.0s/2m0s

running (1m18.0s), 20/20 VUs, 1187 complete and 0 interrupted iterations
default   [  65% ] 20 VUs  1m18.0s/2m0s

running (1m19.0s), 20/20 VUs, 1200 complete and 0 interrupted iterations
default   [  66% ] 20 VUs  1m19.0s/2m0s

running (1m20.0s), 20/20 VUs, 1216 complete and 0 interrupted iterations
default   [  67% ] 20 VUs  1m20.0s/2m0s

running (1m21.0s), 20/20 VUs, 1232 complete and 0 interrupted iterations
default   [  67% ] 20 VUs  1m21.0s/2m0s

running (1m22.0s), 20/20 VUs, 1248 complete and 0 interrupted iterations
default   [  68% ] 20 VUs  1m22.0s/2m0s

running (1m23.0s), 20/20 VUs, 1261 complete and 0 interrupted iterations
default   [  69% ] 20 VUs  1m23.0s/2m0s

running (1m24.0s), 20/20 VUs, 1277 complete and 0 interrupted iterations
default   [  70% ] 20 VUs  1m24.0s/2m0s

running (1m25.0s), 20/20 VUs, 1291 complete and 0 interrupted iterations
default   [  71% ] 20 VUs  1m25.0s/2m0s

running (1m26.0s), 20/20 VUs, 1309 complete and 0 interrupted iterations
default   [  72% ] 20 VUs  1m26.0s/2m0s

running (1m27.0s), 20/20 VUs, 1323 complete and 0 interrupted iterations
default   [  72% ] 20 VUs  1m27.0s/2m0s

running (1m28.0s), 20/20 VUs, 1340 complete and 0 interrupted iterations
default   [  73% ] 20 VUs  1m28.0s/2m0s

running (1m29.0s), 20/20 VUs, 1352 complete and 0 interrupted iterations
default   [  74% ] 20 VUs  1m29.0s/2m0s

running (1m30.0s), 20/20 VUs, 1370 complete and 0 interrupted iterations
default   [  75% ] 20 VUs  1m30.0s/2m0s

running (1m31.0s), 20/20 VUs, 1384 complete and 0 interrupted iterations
default   [  76% ] 20 VUs  1m31.0s/2m0s

running (1m32.0s), 20/20 VUs, 1400 complete and 0 interrupted iterations
default   [  77% ] 20 VUs  1m32.0s/2m0s

running (1m33.0s), 20/20 VUs, 1414 complete and 0 interrupted iterations
default   [  77% ] 20 VUs  1m33.0s/2m0s

running (1m34.0s), 20/20 VUs, 1430 complete and 0 interrupted iterations
default   [  78% ] 20 VUs  1m34.0s/2m0s

running (1m35.0s), 20/20 VUs, 1446 complete and 0 interrupted iterations
default   [  79% ] 20 VUs  1m35.0s/2m0s

running (1m36.0s), 20/20 VUs, 1461 complete and 0 interrupted iterations
default   [  80% ] 20 VUs  1m36.0s/2m0s

running (1m37.0s), 20/20 VUs, 1478 complete and 0 interrupted iterations
default   [  81% ] 20 VUs  1m37.0s/2m0s

running (1m38.0s), 20/20 VUs, 1492 complete and 0 interrupted iterations
default   [  82% ] 20 VUs  1m38.0s/2m0s

running (1m39.0s), 20/20 VUs, 1506 complete and 0 interrupted iterations
default   [  82% ] 20 VUs  1m39.0s/2m0s

running (1m40.0s), 20/20 VUs, 1521 complete and 0 interrupted iterations
default   [  83% ] 20 VUs  1m40.0s/2m0s
time="2026-09-22T21:35:43+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:43+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:43+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:43+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:43+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:43+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:43+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:43+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:43+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:43+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:43+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:43+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:43+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:43+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:43+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:43+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:43+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:43+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:43+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:43+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:43+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:43+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:43+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:43+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:43+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:43+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:43+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:43+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:43+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:43+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:43+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:43+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:43+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:43+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:43+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:43+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:43+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:43+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:43+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:43+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"

running (1m41.0s), 20/20 VUs, 1539 complete and 0 interrupted iterations
default   [  84% ] 20 VUs  1m41.0s/2m0s
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:44+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"

running (1m42.0s), 20/20 VUs, 1559 complete and 0 interrupted iterations
default   [  85% ] 20 VUs  1m42.0s/2m0s
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:45+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"

running (1m43.0s), 20/20 VUs, 1579 complete and 0 interrupted iterations
default   [  86% ] 20 VUs  1m43.0s/2m0s
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:46+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"

running (1m44.0s), 20/20 VUs, 1599 complete and 0 interrupted iterations
default   [  87% ] 20 VUs  1m44.0s/2m0s
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:47+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"

running (1m45.0s), 20/20 VUs, 1619 complete and 0 interrupted iterations
default   [  87% ] 20 VUs  1m45.0s/2m0s
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:48+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"

running (1m46.0s), 20/20 VUs, 1639 complete and 0 interrupted iterations
default   [  88% ] 20 VUs  1m46.0s/2m0s
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:49+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"

running (1m47.0s), 20/20 VUs, 1659 complete and 0 interrupted iterations
default   [  89% ] 20 VUs  1m47.0s/2m0s
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:50+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"

running (1m48.0s), 20/20 VUs, 1679 complete and 0 interrupted iterations
default   [  90% ] 20 VUs  1m48.0s/2m0s
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:51+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"

running (1m49.0s), 20/20 VUs, 1699 complete and 0 interrupted iterations
default   [  91% ] 20 VUs  1m49.0s/2m0s
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:52+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"

running (1m50.0s), 20/20 VUs, 1719 complete and 0 interrupted iterations
default   [  92% ] 20 VUs  1m50.0s/2m0s
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:53+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"

running (1m51.0s), 20/20 VUs, 1739 complete and 0 interrupted iterations
default   [  92% ] 20 VUs  1m51.0s/2m0s
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:54+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"

running (1m52.0s), 20/20 VUs, 1759 complete and 0 interrupted iterations
default   [  93% ] 20 VUs  1m52.0s/2m0s
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:55+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:56+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:56+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:56+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:56+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:56+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:56+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:56+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:56+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:56+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:56+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:56+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:56+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:56+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:56+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:56+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"

running (1m53.0s), 20/20 VUs, 1779 complete and 0 interrupted iterations
default   [  94% ] 20 VUs  1m53.0s/2m0s
time="2026-09-22T21:35:56+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:56+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:56+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:56+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:56+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:56+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:56+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:56+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:56+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:56+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:56+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:56+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:56+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:56+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:56+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:56+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:56+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:56+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:56+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:56+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:56+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:56+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:56+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:56+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:56+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:56+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:56+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:56+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:56+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:56+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:56+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:56+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:56+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:56+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:56+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:56+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:56+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:56+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:56+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:56+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:56+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:56+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"

running (1m54.0s), 20/20 VUs, 1799 complete and 0 interrupted iterations
default   [  95% ] 20 VUs  1m54.0s/2m0s
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:57+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"

running (1m55.0s), 20/20 VUs, 1819 complete and 0 interrupted iterations
default   [  96% ] 20 VUs  1m55.0s/2m0s
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:58+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"

running (1m56.0s), 20/20 VUs, 1839 complete and 0 interrupted iterations
default   [  97% ] 20 VUs  1m56.0s/2m0s
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:35:59+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"

running (1m57.0s), 20/20 VUs, 1859 complete and 0 interrupted iterations
default   [  97% ] 20 VUs  1m57.0s/2m0s
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:00+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"

running (1m58.0s), 20/20 VUs, 1879 complete and 0 interrupted iterations
default   [  98% ] 20 VUs  1m58.0s/2m0s
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:01+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"

running (1m59.0s), 20/20 VUs, 1899 complete and 0 interrupted iterations
default   [  99% ] 20 VUs  1m59.0s/2m0s
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:02+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:03+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:03+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:03+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:03+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:03+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:03+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:03+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:03+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:03+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:03+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:03+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:03+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:03+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:03+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:03+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:03+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/cart/add\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:03+08:00" level=warning msg="Request Failed" error="Get \"http://localhost:3000/report\": dial tcp 127.0.0.1:3000: connect: connection refused"
time="2026-09-22T21:36:03+08:00" level=warning msg="Request Failed" error="Post \"http://localhost:3000/pay\": dial tcp 127.0.0.1:3000: connect: connection refused"

running (2m00.0s), 20/20 VUs, 1919 complete and 0 interrupted iterations
default   [ 100% ] 20 VUs  2m00.0s/2m0s


  █ THRESHOLDS 

    checks
    ✗ 'rate>0.90' rate=77.89%

    http_req_duration{name:cart}
    ✓ 'p(95)<200' p(95)=2.11ms

    http_req_duration{name:report}
    ✓ 'p(95)<450' p(95)=387.62ms

    http_req_failed{name:pay}
    ✗ 'rate<0.08' rate=25.27%


  █ TOTAL RESULTS 

    checks_total.......: 5817   48.088914/s
    checks_succeeded...: 77.89% 4531 out of 5817
    checks_failed......: 22.10% 1286 out of 5817

    ✗ cart 200
      ↳  79% — ✓ 1542 / ✗ 397
    ✗ report 200
      ↳  79% — ✓ 1540 / ✗ 399
    ✗ pay 200
      ↳  74% — ✓ 1449 / ✗ 490

    HTTP
    http_req_duration..............: avg=80.42ms  min=0s       med=1.22ms   max=401.16ms p(90)=326.43ms p(95)=362.73ms
      { expected_response:true }...: avg=103.18ms min=154.68µs med=1.48ms   max=401.16ms p(90)=343.19ms p(95)=370.85ms
      { name:cart }................: avg=1.1ms    min=0s       med=1.17ms   max=4.99ms   p(90)=1.9ms    p(95)=2.11ms  
      { name:report }..............: avg=239.33ms min=0s       med=275.88ms max=401.16ms p(90)=374.1ms  p(95)=387.62ms
    http_req_failed................: 22.10% 1286 out of 5817
      { name:pay }.................: 25.27% 490 out of 1939
    http_reqs......................: 5817   48.088914/s

    EXECUTION
    iteration_duration.............: avg=1.24s    min=1s       med=1.27s    max=1.4s     p(90)=1.37s    p(95)=1.39s   
    iterations.....................: 1939   16.029638/s
    vus............................: 20     min=20           max=20
    vus_max........................: 20     min=20           max=20

    NETWORK
    data_received..................: 1.2 MB 9.6 kB/s
    data_sent......................: 412 kB 3.4 kB/s




running (2m01.0s), 00/20 VUs, 1939 complete and 0 interrupted iterations
default ✓ [ 100% ] 20 VUs  2m0s
time="2026-09-22T21:36:04+08:00" level=error msg="thresholds on metrics 'checks, http_req_failed{name:pay}' have been crossed"
```

## k6 үр дүн — FAIL (results/fail.txt)
```

         /\      Grafana   /‾‾/  
    /\  /  \     |\  __   /  /   
   /  \/    \    | |/ /  /   ‾‾\ 
  /          \   |   (  |  (‾)  |
 / __________ \  |_|\_\  \_____/ 


     execution: local
        script: slo-test-fail.js
        output: -

     scenarios: (100.00%) 1 scenario, 20 max VUs, 1m30s max duration (incl. graceful stop):
              * default: 20 looping VUs for 1m0s (gracefulStop: 30s)


running (0m01.0s), 20/20 VUs, 0 complete and 0 interrupted iterations
default   [   2% ] 20 VUs  0m01.0s/1m0s

running (0m02.0s), 20/20 VUs, 20 complete and 0 interrupted iterations
default   [   3% ] 20 VUs  0m02.0s/1m0s

running (0m03.0s), 20/20 VUs, 40 complete and 0 interrupted iterations
default   [   5% ] 20 VUs  0m03.0s/1m0s

running (0m04.0s), 20/20 VUs, 57 complete and 0 interrupted iterations
default   [   7% ] 20 VUs  0m04.0s/1m0s

running (0m05.0s), 20/20 VUs, 62 complete and 0 interrupted iterations
default   [   8% ] 20 VUs  0m05.0s/1m0s

running (0m06.0s), 20/20 VUs, 80 complete and 0 interrupted iterations
default   [  10% ] 20 VUs  0m06.0s/1m0s

running (0m07.0s), 20/20 VUs, 100 complete and 0 interrupted iterations
default   [  12% ] 20 VUs  0m07.0s/1m0s

running (0m08.0s), 20/20 VUs, 120 complete and 0 interrupted iterations
default   [  13% ] 20 VUs  0m08.0s/1m0s

running (0m09.0s), 20/20 VUs, 125 complete and 0 interrupted iterations
default   [  15% ] 20 VUs  0m09.0s/1m0s

running (0m10.0s), 20/20 VUs, 140 complete and 0 interrupted iterations
default   [  17% ] 20 VUs  0m10.0s/1m0s

running (0m11.0s), 20/20 VUs, 160 complete and 0 interrupted iterations
default   [  18% ] 20 VUs  0m11.0s/1m0s

running (0m12.0s), 20/20 VUs, 180 complete and 0 interrupted iterations
default   [  20% ] 20 VUs  0m12.0s/1m0s

running (0m13.0s), 20/20 VUs, 190 complete and 0 interrupted iterations
default   [  22% ] 20 VUs  0m13.0s/1m0s

running (0m14.0s), 20/20 VUs, 202 complete and 0 interrupted iterations
default   [  23% ] 20 VUs  0m14.0s/1m0s

running (0m15.0s), 20/20 VUs, 220 complete and 0 interrupted iterations
default   [  25% ] 20 VUs  0m15.0s/1m0s

running (0m16.0s), 20/20 VUs, 240 complete and 0 interrupted iterations
default   [  27% ] 20 VUs  0m16.0s/1m0s

running (0m17.0s), 20/20 VUs, 252 complete and 0 interrupted iterations
default   [  28% ] 20 VUs  0m17.0s/1m0s

running (0m18.0s), 20/20 VUs, 262 complete and 0 interrupted iterations
default   [  30% ] 20 VUs  0m18.0s/1m0s

running (0m19.0s), 20/20 VUs, 281 complete and 0 interrupted iterations
default   [  32% ] 20 VUs  0m19.0s/1m0s

running (0m20.0s), 20/20 VUs, 300 complete and 0 interrupted iterations
default   [  33% ] 20 VUs  0m20.0s/1m0s

running (0m21.0s), 20/20 VUs, 317 complete and 0 interrupted iterations
default   [  35% ] 20 VUs  0m21.0s/1m0s

running (0m22.0s), 20/20 VUs, 325 complete and 0 interrupted iterations
default   [  37% ] 20 VUs  0m22.0s/1m0s

running (0m23.0s), 20/20 VUs, 341 complete and 0 interrupted iterations
default   [  38% ] 20 VUs  0m23.0s/1m0s

running (0m24.0s), 20/20 VUs, 360 complete and 0 interrupted iterations
default   [  40% ] 20 VUs  0m24.0s/1m0s

running (0m25.0s), 20/20 VUs, 379 complete and 0 interrupted iterations
default   [  42% ] 20 VUs  0m25.0s/1m0s

running (0m26.0s), 20/20 VUs, 386 complete and 0 interrupted iterations
default   [  43% ] 20 VUs  0m26.0s/1m0s

running (0m27.0s), 20/20 VUs, 403 complete and 0 interrupted iterations
default   [  45% ] 20 VUs  0m27.0s/1m0s

running (0m28.0s), 20/20 VUs, 420 complete and 0 interrupted iterations
default   [  47% ] 20 VUs  0m28.0s/1m0s

running (0m29.0s), 20/20 VUs, 439 complete and 0 interrupted iterations
default   [  48% ] 20 VUs  0m29.0s/1m0s

running (0m30.0s), 20/20 VUs, 450 complete and 0 interrupted iterations
default   [  50% ] 20 VUs  0m30.0s/1m0s

running (0m31.0s), 20/20 VUs, 465 complete and 0 interrupted iterations
default   [  52% ] 20 VUs  0m31.0s/1m0s

running (0m32.0s), 20/20 VUs, 480 complete and 0 interrupted iterations
default   [  53% ] 20 VUs  0m32.0s/1m0s

running (0m33.0s), 20/20 VUs, 500 complete and 0 interrupted iterations
default   [  55% ] 20 VUs  0m33.0s/1m0s

running (0m34.0s), 20/20 VUs, 513 complete and 0 interrupted iterations
default   [  57% ] 20 VUs  0m34.0s/1m0s

running (0m35.0s), 20/20 VUs, 525 complete and 0 interrupted iterations
default   [  58% ] 20 VUs  0m35.0s/1m0s

running (0m36.0s), 20/20 VUs, 541 complete and 0 interrupted iterations
default   [  60% ] 20 VUs  0m36.0s/1m0s

running (0m37.0s), 20/20 VUs, 560 complete and 0 interrupted iterations
default   [  62% ] 20 VUs  0m37.0s/1m0s

running (0m38.0s), 20/20 VUs, 575 complete and 0 interrupted iterations
default   [  63% ] 20 VUs  0m38.0s/1m0s

running (0m39.0s), 20/20 VUs, 586 complete and 0 interrupted iterations
default   [  65% ] 20 VUs  0m39.0s/1m0s

running (0m40.0s), 20/20 VUs, 603 complete and 0 interrupted iterations
default   [  67% ] 20 VUs  0m40.0s/1m0s

running (0m41.0s), 20/20 VUs, 620 complete and 0 interrupted iterations
default   [  68% ] 20 VUs  0m41.0s/1m0s

running (0m42.0s), 20/20 VUs, 635 complete and 0 interrupted iterations
default   [  70% ] 20 VUs  0m42.0s/1m0s

running (0m43.0s), 20/20 VUs, 649 complete and 0 interrupted iterations
default   [  72% ] 20 VUs  0m43.0s/1m0s

running (0m44.0s), 20/20 VUs, 664 complete and 0 interrupted iterations
default   [  73% ] 20 VUs  0m44.0s/1m0s

running (0m45.0s), 20/20 VUs, 680 complete and 0 interrupted iterations
default   [  75% ] 20 VUs  0m45.0s/1m0s

running (0m46.0s), 20/20 VUs, 697 complete and 0 interrupted iterations
default   [  77% ] 20 VUs  0m46.0s/1m0s

running (0m47.0s), 20/20 VUs, 710 complete and 0 interrupted iterations
default   [  78% ] 20 VUs  0m47.0s/1m0s

running (0m48.0s), 20/20 VUs, 724 complete and 0 interrupted iterations
default   [  80% ] 20 VUs  0m48.0s/1m0s

running (0m49.0s), 20/20 VUs, 741 complete and 0 interrupted iterations
default   [  82% ] 20 VUs  0m49.0s/1m0s

running (0m50.0s), 20/20 VUs, 756 complete and 0 interrupted iterations
default   [  83% ] 20 VUs  0m50.0s/1m0s

running (0m51.0s), 20/20 VUs, 771 complete and 0 interrupted iterations
default   [  85% ] 20 VUs  0m51.0s/1m0s

running (0m52.0s), 20/20 VUs, 788 complete and 0 interrupted iterations
default   [  87% ] 20 VUs  0m52.0s/1m0s

running (0m53.0s), 20/20 VUs, 801 complete and 0 interrupted iterations
default   [  88% ] 20 VUs  0m53.0s/1m0s

running (0m54.0s), 20/20 VUs, 816 complete and 0 interrupted iterations
default   [  90% ] 20 VUs  0m54.0s/1m0s

running (0m55.0s), 20/20 VUs, 833 complete and 0 interrupted iterations
default   [  92% ] 20 VUs  0m55.0s/1m0s

running (0m56.0s), 20/20 VUs, 849 complete and 0 interrupted iterations
default   [  93% ] 20 VUs  0m56.0s/1m0s

running (0m57.0s), 20/20 VUs, 861 complete and 0 interrupted iterations
default   [  95% ] 20 VUs  0m57.0s/1m0s

running (0m58.0s), 20/20 VUs, 879 complete and 0 interrupted iterations
default   [  97% ] 20 VUs  0m58.0s/1m0s

running (0m59.0s), 20/20 VUs, 894 complete and 0 interrupted iterations
default   [  98% ] 20 VUs  0m59.0s/1m0s

running (1m00.0s), 20/20 VUs, 911 complete and 0 interrupted iterations
default   [ 100% ] 20 VUs  1m00.0s/1m0s

running (1m01.0s), 08/20 VUs, 923 complete and 0 interrupted iterations
default ↓ [ 100% ] 20 VUs  1m0s


  █ THRESHOLDS 

    checks
    ✓ 'rate>0.90' rate=98.24%

    http_req_duration{name:cart}
    ✓ 'p(95)<200' p(95)=2.64ms

    http_req_duration{name:report}
    ✗ 'p(95)<100' p(95)=393.83ms

    http_req_failed{name:pay}
    ✓ 'rate<0.08' rate=5.26%


  █ TOTAL RESULTS 

    checks_total.......: 2793   45.616197/s
    checks_succeeded...: 98.24% 2744 out of 2793
    checks_failed......: 1.75%  49 out of 2793

    ✓ cart 200
    ✓ report 200
    ✗ pay 200
      ↳  94% — ✓ 882 / ✗ 49

    HTTP
    http_req_duration..............: avg=101.52ms min=285.47µs med=1.75ms  max=402.72ms p(90)=343.98ms p(95)=374.94ms
      { expected_response:true }...: avg=103.31ms min=285.47µs med=1.77ms  max=402.72ms p(90)=345.03ms p(95)=375.37ms
      { name:cart }................: avg=1.7ms    min=296.7µs  med=1.7ms   max=4.5ms    p(90)=2.31ms   p(95)=2.64ms  
      { name:report }..............: avg=301.62ms min=201.07ms med=300.4ms max=402.72ms p(90)=384.13ms p(95)=393.83ms
    http_req_failed................: 1.75%  49 out of 2793
      { name:pay }.................: 5.26%  49 out of 931
    http_reqs......................: 2793   45.616197/s

    EXECUTION
    iteration_duration.............: avg=1.3s     min=1.2s     med=1.3s    max=1.4s     p(90)=1.38s    p(95)=1.39s   
    iterations.....................: 931    15.205399/s
    vus............................: 8      min=8          max=20
    vus_max........................: 20     min=20         max=20

    NETWORK
    data_received..................: 701 kB 11 kB/s
    data_sent......................: 249 kB 4.1 kB/s




running (1m01.2s), 00/20 VUs, 931 complete and 0 interrupted iterations
default ✓ [ 100% ] 20 VUs  1m0s
time="2026-09-22T21:43:11+08:00" level=error msg="thresholds on metrics 'http_req_duration{name:report}' have been crossed"
k6 exit code: 99
```
## Дүгнэлт

Энэ лабораторийн хамгийн хэцүү хэсэг нь сценариогийн чанарын шаардлагыг тоон SLO болгон хувиргах, тэр тоог k6 threshold код руу яг тохируулан бичих явдал байлаа. Ялангуяа Reliability болон Availability хоёрын ялгааг ойлгоход цаг зарцуулсан — сервер унасан тохиолдолд /pay endpoint-ийн бүх хүсэлт амжилтгүй болдог тул нэг л эвдрэл хоёр өөр SLO-г зэрэг зөрчиж болохыг chaos тестээс тодорхой харлаа. PASS тестэд бүх 4 threshold амжилттай биелсэн бол (checks 98.05%, cart p95=2.93мс, report p95=394мс, pay error rate=5.83%), chaos тест үед сервер 10 секунд унасны улмаас checks болон pay reliability threshold хоёулаа FAIL болсон. Энэ нь миний availability сценариог батлаагүй харин эсрэгээрээ — availability SLO маань хэт өөдрөг байсныг, эсвэл 10 секундийн зогсолт хэт их байсныг харууллаа. Цагаар тооцсон 12 секундийн error budget-тэй харьцуулахад, хүсэлтээр тооцсон бодит алдааны хувь хамаагүй өндөр гарсан нь k6-ийн checks нь цагийн бус хүсэлтийн хувиар хэмждэгтэй холбоотой — сервер унасан үед хүсэлтүүд агшин зуур амжилтгүй болдог тул зогсолтын 1 секунд дотор олон хүсэлт "иддэг". Threshold-оо зориуд эвдэхийн тулд /report-ын p95 хязгаарыг 100мс болгосноор яг FAIL болсныг баталгаажуулж, exit code 99 (0 биш) гарсныг харлаа — энэ нь яг CI/CD pipeline дээр build-ыг зогсоодог механизм гэдгийг ойлголоо. Ерөнхийдөө энэ лаб надад чанарын шаардлагыг зөвхөн тайлбарлахаас илүү, автоматаар шалгагдах хэмжигдэхүүн болгон хувиргаж сурахад тусалсан.

_(Lab бэлдэгдсэн: server → scenarios → SLO → k6 threshold → chaos → fail test)_

_(GitHub public repo, results/ folder бэлэн)_
