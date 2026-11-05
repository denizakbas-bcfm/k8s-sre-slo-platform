# Kubernetes SLO-Driven Monitoring & Error Budget Platform

Bu proje; SRE (Site Reliability Engineering) prensiplerini uygulayarak, Kubernetes üzerinde çalışan bir web servisinin erişilebilirliğini anlık alarmlar yerine **SLI/SLO** ve **Rolling-Window Hata Bütçesi (Error Budget)** metrikleri üzerinden izleyen uçtan uca güvenilirlik mimarisidir.

---

## 🎯 SRE Metodolojisi & Parametreler

Geleneksel izleme yaklaşımlarındaki anlık dalgalanma (flapping) ve alarm yorgunluğunu (alert fatigue) önlemek amacıyla Google SRE pratiklerine uygun bir yapı kurgulanmıştır:

- **SLI (Service Level Indicator):** Son 5 dakikadaki başarılı HTTP kontrollerinin toplam kontrollere oranı.
  $$\text{SLI} = \frac{\text{sum\_over\_time(probe\_success[5m])}}{\text{count\_over\_time(probe\_success[5m])}}$$
- **SLO (Service Level Objective):** Son 5 dakikalık pencerede **%95** kullanılabilirlik hedefi.
- **Hata Bütçesi (Error Budget):** %5 kesinti payı.
- **Alarm Kuralı:** Başarı oranı 30 saniye boyunca %95'in altında kaldığında devreye giren SLO alarmı (`WebAppSLOBreach`).
- **Ölçeklenebilirlik (Autoscaling):** Hedef web servisi, CPU yüküne göre 2 ile 4 replika arasında otomatik ölçeklenecek şekilde `HorizontalPodAutoscaler` (HPA) ile korunmaktadır.
