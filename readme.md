# Background

[small file challenges](https://blog.cloudera.com/small-files-big-foils-addressing-the-associated-metadata-and-application-challenges/)

# How to run kompactor
```bash
spark2-submit --master yarn --deploy-mode client --driver-memory 2g --executor-memory 2g --executor-cores 2 --files logging.ini kompactor.py --table_name bdp_ap_it.music_service_raw
```