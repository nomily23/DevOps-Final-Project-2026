# DevOps Final Project - Task Manager

## Project Overview
פרויקט זה מציג מערכת ניהול משימות (Task Manager) מבוססת מיקרו-שירותים. [cite_start]המערכת כוללת ממשק משתמש, שרת API לניהול בקשות ומסד נתונים מסוג PostgreSQL, תוך שימוש בכלי DevOps מתקדמים לאוטומציה ופריסה[cite: 6, 7, 8, 9].

## Architecture
המערכת בנויה משלוש שכבות:
1. [cite_start]**Frontend:** ממשק משתמש אינטראקטיבי[cite: 7].
2. [cite_start]**Backend:** שרת API המטפל בלוגיקה העסקית[cite: 8].
3. [cite_start]**Database:** מסד נתונים PostgreSQL להבטחת עמידות נתונים (Persistence)[cite: 9, 30].

## CI/CD Process
[cite_start]תהליך ה-CI/CD מבוצע באמצעות **Jenkins**[cite: 40]. [cite_start]ברגע שמתבצע `git push` ל-Repository, מופעל Webhook המפעיל Pipeline אוטומטי הכולל[cite: 41]:
- [cite_start]בניית Docker Images עבור השירותים[cite: 45].
- [cite_start]דחיפת ה-Images למאגר ה-AWS ECR[cite: 20, 45].
- [cite_start]עדכון אוטומטי של ה-Deployment ב-Kubernetes[cite: 45].

![Webhook Success](images/webhook_success.png)
*איור 1: אינטגרציה עם GitHub - אישור מסירה מוצלח של ה-Webhook המפעיל את ה-Pipeline.*

![Jenkins Pipeline](images/jenkins_pipeline.png)
*איור 2: Pipeline ירוק ותקין ב-Jenkins.*

## Kubernetes Deployment
[cite_start]האפליקציה מנוהלת באמצעות קבצי YAML המגדירים את הפריסה ב-EKS[cite: 24, 25]:
- [cite_start]**db.yaml:** הגדרת ה-Postgres עם PersistentVolumeClaim (PVC) להבטחת נתונים[cite: 28, 30].
- [cite_start]**backend.yaml:** פריסת ה-Backend עם 3 Replicas ו-Readiness Probe לווידוא תקינות חיבור ה-DB[cite: 26, 32, 33].
- [cite_start]**ingress.yaml:** חשיפת ה-Frontend החוצה באמצעות Ingress[cite: 34].
- [cite_start]**Security:** מידע רגיש (סיסמאות) נוהל באמצעות Secrets מקודדים ב-Base64[cite: 31].

![Kubernetes Pods](images/k8s_pods.png)
*איור 3: סטטוס ה-Pods והשירותים בקלאסטר (kubectl get pods, svc, ingress).*

## Summary & Deployment Strategy
[cite_start]הפריסה לקלאסטר מוגדרת ב-Pipeline באמצעות פקודות `kubectl`[cite: 37, 45]. [cite_start]לצורך חיסכון בעלויות הענן, הקלאסטר הושבת לאחר תהליך הוולידציה, ולכן ה-Pipeline מוגדר להדגים את תהליך העדכון (Deployment logic demonstration)[cite: 46].
