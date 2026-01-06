# edu prometheus-Deployment

بعد از اجرا :
```
kubectl apply -f prometheus-deployment.yaml
```

Service برای دسترسی

```
apiVersion: v1
kind: Service
metadata:
  name: prometheus
spec:
  selector:
    app: prometheus
  ports:
  - port: 9090
    targetPort: 9090
  type: ClusterIP
```

```
kubectl apply -f prometheus-service.yaml
```
4️⃣ دسترسی به Prometheus
Port Forward:

```
kubectl port-forward svc/prometheus 9090:9090
```

مرورگر:
```
http://localhost:9090
```
5️⃣ (پیشنهادی) ذخیره دیتای پایدار با PVC

اگر Pod ریست بشه دیتا از بین می‌ره؛ برای جلوگیری:

PVC:
```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: prometheus-pvc
spec:
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
      storage: 10Gi
```
تغییر Deployment:
```
volumeMounts:
- name: prometheus-data
  mountPath: /prometheus

volumes:
- name: prometheus-data
  persistentVolumeClaim:
    claimName: prometheus-pvc
```

و اضافه کن:
```
args:
- "--storage.tsdb.path=/prometheus"
```
6️⃣ نکات حرفه‌ای / مصاحبه‌ای 🔥
مورد	توضیح
Deployment	چون HA نداریم
ConfigMap	مدیریت config
PVC	حفظ metrics
Service	دسترسی داخلی
Operator	بهترین practice واقعی
