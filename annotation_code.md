Transrate
--

```
transrate -t 20 -o Harmonia.v1.0.1 -a ../Harmonia.v1.0.1.fasta \
-l ../harmonia_adult.R1.fastq.gz,../harmonia_larva.R1.fastq.gz \
-r ../harmonia_adult.R2.fastq.gz,../harmonia_larva.R2.fastq.gz
```

BUSCO
--

```
python3 /share/BUSCO_v1.1b1/BUSCO_v1.1b1.py -o final \
-in Harmonia.v1.0.1.fasta -l arthropoda -c 25
```

kallisto
--

```
kallisto index -i transcripts2.idx ../Harmonia.v1.0.1.fasta

kallisto quant -t 32 -i transcripts2.idx -o kallisto_adult -b 100 \
../harmonia_adult.R1.fastq.gz \
../harmonia_adult.R2.fastq.gz

kallisto quant -t 32 -i transcripts2.idx -o kallisto_larva -b 100 \
../harmonia_larva.R1.fastq.gz \
../harmonia_larvae.R2.fastq.gz

```

Salmon
--

```
salmon index -t ../Harmonia.v1.0.1.fasta -i transcripts2_index --type quasi -k 31

salmon quant -p 32 -i transcripts2_index -l MSR \
-1 <(gzip -cd ../harmonia_adult.R1.fastq.gz) \
-2 <(gzip -cd ../harmonia_adult.R2.fastq.gz) -o salmon_adult

salmon quant -p 32 -i transcripts2_index -l MSR \
-1 <(gzip -cd ../harmonia_larva.R1.fastq.gz) \
-2 <(gzip -cd ../harmonia_larvae.R2.fastq.gz) -o salmon_larva
```

dammit!
--

```
dammit databases --install --busco-group arthopoda --database-dir /mnt/dammit/
dammit annotate Harmonia.v1.0.1.fasta --busco-group arthropoda --n_threads 36 --database-dir /mnt/dammit/  
```

signalP
--

```
signalp -v ../Harmonia.v1.0.1.fasta > Harmonia.v1.0.1.signalP
```
