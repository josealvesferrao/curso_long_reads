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


