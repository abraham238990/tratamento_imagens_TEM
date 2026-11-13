# Tratamento e Análise de Imagens de Microscopia Eletrônica de Transmissão (TEM)

<p align="center">
  <img src="images/Cabecalho.png" alt="Cabeçalho" width="100%">
</p>

<p align="center">
  <i>Desenvolvimento de ferramentas computacionais em Python para melhorar imagens, identificar nanopartículas, analisar suas características e estudar a organização das estruturas observadas por microscopia eletrônica.</i>
  <br><br>
  Ilum School of Science | Campinas, 2026
</p>

## Sobre o projeto

Este repositório reúne um conjunto de notebooks desenvolvidos em Python para o processamento e a análise de imagens obtidas por **Microscopia Eletrônica de Transmissão (TEM)**.

O objetivo é desenvolver ferramentas capazes de melhorar a qualidade das imagens, identificar estruturas de interesse, detectar nanopartículas e extrair informações sobre suas características e distribuição espacial.

O projeto utiliza imagens convencionais de microscopia eletrônica em duas dimensões (2D), como arquivos `.tif` de alta resolução, a exemplo da imagem `CG_059.tif`, com resolução de 2048 × 2048 pixels.

Uma característica importante é que os métodos foram adaptados para trabalhar diretamente com essas imagens, sem exigir conjuntos de dados mais complexos, como os utilizados em técnicas avançadas de microscopia 4D-STEM, padrões de difração ou arquivos de estruturas cristalinas (CIF).

O projeto está organizado em **dois pipelines complementares**:

- **SCIENTIFIC:** voltado à análise quantitativa, priorizando a preservação das informações presentes na imagem original. É indicado para medições e análises científicas.
- **VISUAL:** voltado à melhoria da aparência das imagens, aumentando o contraste, a nitidez e a definição para facilitar a observação e a apresentação dos resultados.

Essa separação permite trabalhar tanto com imagens adequadas para análises científicas quanto com versões visualmente aprimoradas para apresentação e documentação.

---

## Estrutura e Pipelines dos Notebooks

### 1. Restauração e Realce de Imagem (`LLTO_infocus`)

Este pipeline tem como objetivo melhorar a qualidade das imagens originais, corrigindo problemas de iluminação, removendo ruídos e recuperando detalhes que podem estar pouco definidos.

Entre as principais etapas estão:

- **Calibração e identificação da barra de escala:** identificação automática da barra de escala presente na imagem, permitindo estabelecer a relação entre pixels e unidades de comprimento, como nanômetros (`nm/pixel`). A região da barra é excluída das análises e preenchida de maneira compatível com o fundo da imagem.

- **Remoção de ruídos e correção de iluminação:** identificação e correção de pixels muito brilhantes ou defeituosos, que podem aparecer devido a interferências durante a aquisição. Também é realizada uma correção da iluminação para reduzir diferenças de brilho entre regiões da imagem.

- **Análise de Fourier (FFT):** análise das frequências presentes na imagem para identificar padrões, separar informações úteis de ruídos e estimar os limites de resolução espacial.

- **Simulação de detectores virtuais (BF e ADF):** aplicação de filtros matemáticos para produzir diferentes formas de visualização da imagem. O modo Bright Field (BF) destaca informações associadas às regiões mais claras, enquanto o modo Annular Dark Field (ADF) permite observar contrastes associados a determinadas estruturas.

- **Contraste de Fase Diferencial (DPC) e estimativa de espessura:** análise das variações de intensidade da imagem para construir mapas que representam mudanças espaciais no sinal. Também é calculada uma estimativa de espessura relativa, baseada na intensidade registrada.

- **Restauração de imagem (Richardson-Lucy):** aplicação de um método iterativo de restauração para tentar recuperar detalhes que foram suavizados ou espalhados durante a formação da imagem. O processo utiliza uma estimativa matemática de como o microscópio modifica o sinal original e inclui uma etapa de regularização para evitar o aumento excessivo de ruídos.

### 2. Detecção, Quantificação e Mapeamento (`260717-AlN_A`)

Este pipeline é voltado à identificação automática de nanopartículas e à análise de suas características individuais e da maneira como estão distribuídas na imagem.

As principais etapas são:

- **Preparação e melhoria das imagens:** aplicação de diferentes técnicas para ajustar brilho, contraste e nitidez, além de reduzir ruídos. São geradas duas versões: uma voltada à análise científica (`SCIENTIFIC`) e outra à visualização (`VISUAL`).

- **Detecção automática de nanopartículas:** utilização de filtros matemáticos para identificar estruturas que apresentam características semelhantes às de nanopartículas. O sistema estima o tamanho típico das partículas e utiliza modelos de referência para localizar outras estruturas semelhantes na imagem.

- **Localização e caracterização das partículas:** refinamento da posição de cada partícula detectada, incluindo a estimativa de seu centro, tamanho e contraste em relação ao fundo.

- **Análise da distribuição espacial:** estudo da posição das partículas e das distâncias entre elas. Também é calculada a função de distribuição radial, \(g(r)\), que permite investigar se as partículas estão distribuídas aleatoriamente, agrupadas ou organizadas segundo algum padrão espacial.

- **Separação e análise de estruturas maiores:** identificação de regiões maiores e de agrupamentos de partículas utilizando técnicas de segmentação. O método Watershed, por exemplo, permite separar regiões que estão próximas ou conectadas.

- **Análise de orientação e formato:** comparação das estruturas identificadas com modelos matemáticos de elipses, permitindo estimar sua orientação e proporção entre comprimento e largura.

- **Análise de bordas e orientação local:** cálculo de informações sobre a direção predominante das bordas e interfaces presentes na imagem. Isso permite construir mapas que mostram como as estruturas estão orientadas em diferentes regiões.

---

## Fluxo Geral de Processamento

O processamento das imagens é dividido em cinco etapas principais:

| Etapa | O que é realizado | Principais resultados |
|---|---|---|
| **1. Calibração e limpeza** | Identificação da barra de escala, conversão de pixels para nanômetros, remoção de ruídos e correção de iluminação. | Imagem calibrada e corrigida. |
| **2. Realce e filtragem** | Ajustes de brilho, contraste, nitidez e redução de ruídos, com versões para análise e visualização. | Imagens `*_PLUS_SCIENTIFIC.tif` e `*_PLUS_VISUAL.tif`. |
| **3. Restauração e análise** | Análise das frequências, aplicação de filtros, construção de mapas de contraste e restauração de detalhes. | Imagens `*_DPC.tif`, `*_ESPESSURA_RELATIVA.tif` e `*_RESTAURADA_*.tif`. |
| **4. Detecção e quantificação** | Identificação das nanopartículas, medição de suas características e análise de sua distribuição espacial. | Tabela `*_particulas.csv` e gráficos de distribuição. |
| **5. Análise morfológica** | Separação de estruturas maiores, análise de formatos, orientação e características das bordas. | Tabela `*_particulas_grandes.csv`, imagens `*_ROTULOS.tif` e mapas de orientação. |

---

## Tecnologias utilizadas

O projeto utiliza bibliotecas da linguagem Python voltadas à manipulação de imagens, cálculos matemáticos e visualização de resultados.

| Biblioteca | Aplicação |
|---|---|
| `Python` | Linguagem de programação utilizada para desenvolver os pipelines. |
| `NumPy` | Manipulação de imagens como matrizes de números e realização de cálculos matemáticos. |
| `SciPy` | Aplicação de filtros, cálculos de distâncias, operações matemáticas e processamento de imagens. |
| `scikit-image` | Ferramentas para segmentação, identificação de estruturas, redução de ruídos e restauração de imagens. |
| `tifffile` | Leitura e gravação de imagens no formato TIFF, preservando sua precisão numérica e informações disponíveis. |
| `Matplotlib` | Criação de gráficos, visualização das imagens e geração de mapas de cores. |
| `OpenCV (cv2)` | Biblioteca opcional utilizada para acelerar operações de processamento, redução de ruídos e melhoria de contraste. |
| `py4DSTEM` | Biblioteca opcional para trabalhar com dados de microscopia eletrônica avançada e informações cristalográficas. |

---

## Requisitos e instalação

Para executar os notebooks, recomenda-se utilizar **Python 3.10 ou superior**, juntamente com o JupyterLab ou Jupyter Notebook.

As principais bibliotecas utilizadas podem ser instaladas pelo seguinte comando:

```bash
pip install numpy scipy scikit-image tifffile matplotlib opencv-python jupyter
```

Após a instalação, basta iniciar o Jupyter e abrir o notebook desejado para executar as etapas de processamento e análise.

---

## Considerações finais

Os pipelines foram desenvolvidos para facilitar o processamento e a análise de imagens de microscopia eletrônica, reunindo diferentes ferramentas em fluxos de trabalho organizados.

Além de melhorar a qualidade visual das imagens, o projeto busca transformar informações presentes nas micrografias em dados quantitativos, permitindo estudar o tamanho, o formato, a orientação e a distribuição das estruturas observadas.

Dessa maneira, o conjunto de ferramentas pode auxiliar tanto na preparação de imagens para apresentação quanto na investigação de características morfológicas e espaciais de materiais em escala nanométrica.
