# Tratado previo de los datos

* Localizamos los archivos que no tienen cabecera

```bash
grep -R -m1 "" data | grep -v "domainName" | grep -v "domain_name"
```