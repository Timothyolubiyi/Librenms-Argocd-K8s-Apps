## To Launch, Create an EC2 Server Instance and proceed with below ##


1. Generate a Valid APP_KEY (IMPORTANT)

LibreNMS will not start without a valid APP_KEY.

# Run this once:

docker run --rm librenms/librenms php artisan key:generate --show

# Then update:

APP_KEY: "base64:PASTE_GENERATED_KEY"

2.  Deploy via Argo CD
kubectl apply -f argocd/librenms-app.yaml


# Then: Open Argo CD UI

- Sync librenms

- Watch pods go Healthy

3. Verify Deployment

kubectl get pods -n librenms
kubectl get ingress -n librenms

# Expected pods:

- librenms

- librenms-mariadb

- librenms-rrdcached

- librenms-cron

4. First Login

- URL: http://librenms.example.com

- Default user created during setup

- Complete web installer if prompted


5. Production Hardening (Recommended)

- External MariaDB (RDS / Cloud SQL)

- Enable TLS (cert-manager)

- Use SealedSecrets / External Secrets

- Increase RRD storage (50–100Gi)


## ✅ Result

You now have:

✔ Fully GitOps-managed LibreNMS

✔ Auto-healing via Argo CD

✔ Persistent storage

✔ Scalable & production-ready setup