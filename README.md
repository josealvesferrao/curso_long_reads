# curso_long_reads
Curso de Tecnologias de sequenciação de 3ª geração (NGS) e ensaio de sequenciação de genoma humano

# ANÁLISE BIOINFORMÁTICA
## Introdução à linha de comandos 
Abrir uma janela do browser de internet e aceder a este repositório do GitHub https://github.com/krother/bash_tutorial

No MobaXterm clicar em "Start local terminal" e no terminal (substituir "grupo1" pelo seu grupo):

```
ssh grupo1@192.168.32.137
```

Na diretoria "home" do seu utilizador: 
```
cd ~
```

Clonar o repositório git:
```
git clone https://github.com/krother/bash_tutorial.git
```

Testar os ambientes "conda". Listar os ambientes disponíveis:
```
conda env list
```
Ativar o ambiente que vai ser usado mais vezes durante o curso (inclui as ferramentas minimap2, sniffles2, nanoplot, samtools)
```
conda activate curso_amb_long_reads
```
Para desativar o ambiente e regressar ao ambiente anterior (base):
```
conda deactivate
```

# ANÁLISE BIOINFORMÁTICA
## Análise primária
Correr o NanoPlot no ficheiro sequencing_summary.txt da respetiva amostra:
```
NanoPlot --threads 1 --outdir . --loglength --N50 --format pdf --summary /mnt/sdb/long_reads_curso/raw_data/EOL1/sequencing_summary.txt --plots kde 
```


# ANÁLISE BIOINFORMÁTICA
## Mapeamento de reads
Visualizar a estrutura do ficheiro fastq (reads):
```
cd /mnt/sdb/long_reads_curso/raw_data/EOL1/
head -n 4 EOL1_chr15.fastq
```

Correr a ferramenta Minimap2 (ver menu de opções/argumentos):
```
conda activate curso_amb_long_reads
minimap2 --help
```
###Mapeamento amostra EOL1
Efetuar o mapeamento das reads a partir do ficheiro EOL1_chr15.fastq (sequência de referência: chr15.fasta; adaptar diretório de output (opção "-o" de acordo com o seu grupo1,2,3...):
```
minimap2 -a -x map-ont -t 1 -o /mnt/sdb/long_reads_curso/grupo1/EOL1_chr15.sam -R '@RG\tID:flowcell1\tLB:EOL1\tPL:ONT\tSM:EOL1' /mnt/sdb/long_reads_curso/raw_data/fasta_ref_genome/chr15.fasta /mnt/sdb/long_reads_curso/raw_data/EOL1/EOL1_chr15.fastq
```

###Mapeamento amostra 697
Efetuar o mapeamento das reads a partir do ficheiro 697_chr12.fastq (sequência de referência: chr12.fasta; adaptar diretório de output (opção "-o" de acordo com o seu grupo1,2,3...):
```
minimap2 -a -x map-ont -t 1 -o /mnt/sdb/long_reads_curso/grupo1/697_chr12.sam -R '@RG\tID:flowcell1\tLB:697\tPL:ONT\tSM:697' /mnt/sdb/long_reads_curso/raw_data/fasta_ref_genome/chr12.fasta /mnt/sdb/long_reads_curso/raw_data/697/697_chr12.fastq
```


Visualizar a estrutura do cabeçalho (opção "-H") do ficheiro BAM (reads mapeadas):
```
cd /mnt/sdb/long_reads_curso/raw_data/EOL1/
samtools view -H EOL1_merged_aligned_hg38_sorted.bam | less
``` 

Visualizar secção de alinhamento no ficheiro BAM (reads mapeadas):
```
samtools view EOL1_merged_aligned_hg38_sorted.bam | less
```

Abrir o software IGV no servidor:
```
conda activate curso_amb_igv
igv
```

Converter formato SAM para BAM, ordenar por coordenada genómica e indexar:
```
conda activate curso_amb
samtools view -b -o /mnt/sdb/long_reads_curso/grupo1/EOL1_chr15.bam /mnt/sdb/long_reads_curso/grupo1/EOL1_chr15.sam
samtools sort -o /mnt/sdb/long_reads_curso/grupo1/EOL1_chr15_sorted.bam /mnt/sdb/long_reads_curso/grupo1/EOL1_chr15.bam
samtools index /mnt/sdb/long_reads_curso/grupo1/EOL1_chr15_sorted.bam
```

# ANÁLISE BIOINFORMÁTICA
## Chamada e filtragem de variantes
Efetuar a chamada de variantes estruturais com o Sniffles2:
```
conda activate curso_amb_long_reads
sniffles --input /mnt/sdb/long_reads_curso/grupo1/EOL1_chr15.bam --sample-id EOL1_Chr15_LRCourse --qc-output-all --minsvlen 10000 --reference /mnt/sdb/long_reads_curso/raw_data/fasta_ref_genome/chr15.fasta --symbolic --vcf EOL1_chr15.vcf
```

Efetuar filtragem de variantes com o bcftools:
```
conda activate base

bcftools view -i 'FILTER="PASS" && INFO/SUPPORT >= 2' /mnt/sdb/long_reads_curso/grupo1/EOL1_chr15.vcf --output /mnt/sdb/long_reads_curso/grupo1/EOL1_chr15_filtered.vcf

bcftools view -e 'SVTYPE = "INV"' /mnt/sdb/long_reads_curso/grupo1/EOL1_chr15.vcf --output /mnt/sdb/long_reads_curso/grupo1/EOL1_chr15_filtered_2.vcf
``` 
