# Processamento de Dados Meteorológicos com Python (PDMP)

![XI SIC & VII SIMCA](assets\banner-xi-sic-2025.png.png)

## XI Simpósio Internacional de Climatologia & VII Seminário Internacional de Meteorologia e Climatologia do Amazonas

**Data:** 18-22 de Agosto de 2025  
**Local:** Auditório SUDAM, Belém-PA  
**Organização:** SBMET, UFPA, UFAM  
**Website:** [https://sic2025.com.br/](https://sic2025.com.br/)

---

## 📋 Sobre o Evento

O XI Simpósio Internacional de Climatologia (XI SIC) é um evento bienal promovido pela Sociedade Brasileira de Meteorologia (SBMET) que reúne a comunidade científica nacional e internacional para discussões sobre clima, sustentabilidade e resiliência. Este evento serve como preparação para as temáticas que serão discutidas durante a COP30, que também será realizada em Belém.

O SIC visa promover, incentivar e divulgar pesquisas nas áreas de meteorologia e climatologia, reunindo a comunidade científica e profissional para um efetivo intercâmbio de informações e conhecimentos.

---

## 🎯 Sobre o Minicurso

Bem-vindos ao minicurso **"Processamento de Dados Meteorológicos com Python"**, parte da programação do XI SIC 2025 e VII SIMCA, em Belém-PA.

Este minicurso explora o módulo **XISIC.py**, desenvolvido especificamente para este evento, que automatiza a extração, o processamento e a conversão de dados meteorológicos históricos do INMET. O objetivo é facilitar análises científicas e operacionais, em linha com as discussões sobre clima e sustentabilidade na Amazônia.

### 👥 Autores

**Elivaldo Carvalho Rocha**
- Mestrando em Gestão de Risco e Desastres Naturais na Amazônia (PPGGRD-IG-UFPA)
- Bacharel em Meteorologia (FAMET-IG-UFPA) com especialização em Agrometeorologia e climatologia (FAMEESP)
- MBA em Geotecnologias, Ciência de Dados Geográficos
- Pós-graduando em Georreferenciamento, Geoprocessamento e Sensoriamento Remoto
- Graduando em Análise e Desenvolvimento de Sistemas (Faculdade Estácio)
- Desenvolvedor Full Stack Python

**Prof. Dr. João de Athaydes Silva Júnior**
- Professor Adjunto da UFPA (FAMET e PPGGRD)
- Coordenador do PPGGRD (2021-2025)
- Doutor em Desenvolvimento Sustentável do Trópico Úmido (UFPA, 2012)
- Mestre em Meteorologia (UFCG, 2008)
- Diretor Administrativo da SBMET (2023-2025)
- Líder do grupo de pesquisa "Clima na Amazônia"
- Especialista em Climatologia Aplicada e eventos extremos

**Contato:** carvalhovaldo@gmail.com

---

## 📦 Módulo XISIC.py

### Sistema de Conversão e Processamento de Dados Meteorológicos INMET

O módulo **XISIC.py** fornece funcionalidades completas para extrair, processar e converter dados históricos meteorológicos do Instituto Nacional de Meteorologia (INMET) do formato CSV para NetCDF utilizando a biblioteca xarray.

### 🌟 Principais Características

- ✅ **Extração automática** de arquivos ZIP contendo dados meteorológicos
- ✅ **Processamento robusto** de CSVs do INMET com diferentes encodings
- ✅ **Conversão para formato NetCDF** com estrutura padronizada CF-1.8
- ✅ **Filtragem por região, UF e código WMO**
- ✅ **Tratamento de dados faltantes** e inconsistências
- ✅ **Preservação de metadados** e atributos geográficos
- ✅ **Suporte a processamento em lote** com controle de progresso
- ✅ **Estrutura de dados otimizada** para análises com xarray

### 📚 Dependências

```python
pandas       # Manipulação e análise de dados tabulares
xarray       # Estruturas de dados multidimensionais para ciências
numpy        # Computação numérica fundamental
pydantic     # Validação de dados e serialização
pathlib      # Manipulação moderna de caminhos de arquivo
xclim        # Índices climatológicos padronizados
geobr        # Dados geoespaciais brasileiros
```

### 🚀 Instalação das Dependências

```bash
pip install geobr xclim pandas xarray numpy pydantic
```

---

## 📖 Guia de Uso

### 1. Importando o Módulo

```python
from XISIC import (
    extract_and_save_csvs,
    convert_csvs_to_netcdf,
    parse_inmet_csv_to_netcdf_robust,
    debug_inmet_file
)
```

### 2. Extração de Dados do ZIP

```python
# Extrair dados do arquivo ZIP
result = extract_and_save_csvs("/path/to/2024.zip", 2024)
print(f"Arquivos extraídos: {result.file_count}")
```

### 3. Conversão CSV para NetCDF

#### Conversão de Arquivo Único
```python
# Converter um arquivo específico
dataset = parse_inmet_csv_to_netcdf_robust(
    csv_file_path="INMET_CO_DF_A001_BRASILIA_01-01-2024_A_31-12-2024.CSV",
    output_netcdf_path="brasilia_2024.nc",
    debug=True
)
```

#### Conversão em Lote com Filtros
```python
# Converter todos os arquivos
result = convert_csvs_to_netcdf("/content/INMET_2024/CSV", all_files=True)

# Converter apenas Brasília
result = convert_csvs_to_netcdf("/content/INMET_2024/CSV", uf="DF")

# Converter região Centro-Oeste
result = convert_csvs_to_netcdf("/content/INMET_2024/CSV", region="CO")

# Múltiplas UFs
result = convert_csvs_to_netcdf("/content/INMET_2024/CSV", uf=["DF", "SP", "RJ"])

# Combinação de filtros
result = convert_csvs_to_netcdf(
    "/content/INMET_2024/CSV",
    region="CO", 
    uf=["DF", "GO"]
)
```

### 4. Análise dos Dados Convertidos

```python
import xarray as xr
import matplotlib.pyplot as plt

# Carregar dataset NetCDF
ds = xr.open_dataset("brasilia_2024.nc")

# Análises básicas
precip_diaria = ds['PRECIPITAÇÃO TOTAL, HORÁRIO (mm)'].sum(dim='hour_utc')
temp_media = ds['TEMPERATURA DO AR - BULBO SECO, HORARIA (°C)'].mean(dim='hour_utc')

# Visualização
precip_diaria.plot()
plt.title("Precipitação Diária - Brasília 2024")
plt.show()
```

---

## 🔬 Análises Avançadas Implementadas

### 1. Análise Climatológica Básica
- Precipitação acumulada mensal
- Temperaturas médias, máximas e mínimas
- Ciclo diário de temperatura
- Análise de amplitude térmica

### 2. Correlações Meteorológicas
- Matriz de correlação entre variáveis
- Identificação de correlações significativas
- Análise multivariada com critérios estatísticos

### 3. Análise de Extremos com xclim
- Dias consecutivos secos e úmidos
- Múltiplos limiares de análise
- Índices climatológicos padronizados CF Conventions
- Comparação com climatologia normal

### 4. Análise Espacial
- Mapas de precipitação mensal
- Análise multi-estações
- Visualização georreferenciada com shapefile do DF

### 5. Rosa dos Ventos
- Análise de direção e velocidade do vento
- Distribuição por quadrantes
- Visualização polar profissional

---

## 📊 Exemplos de Resultados

### Estrutura do Dataset NetCDF
```
xarray.Dataset
Dimensions: (region: 1, uf: 1, wmo_code: 1, date: 366, hour_utc: 24, lat: 1, lon: 1, alt: 1)
Coordinates:
  * date        (date) datetime64[ns] 2024-01-01 ... 2024-12-31
  * hour_utc    (hour_utc) int32 0 1 2 3 4 5 6 ... 18 19 20 21 22 23
  * lat         (lat) float64 -15.79
  * lon         (lon) float64 -47.93
  * alt         (alt) float64 1.161e+03
Data variables: (17)
```

### Estatísticas Climatológicas (Brasília 2024)
```
📍 Localização: -15.79°S, 47.93°W
⛰️ Altitude: 1161 metros
💧 Precipitação total anual: 1399 mm
🌡 Temperatura média anual: 22.0°C
💨 Velocidade média dos ventos: 2.1 m/s
💧 Umidade relativa média: 67%
🧭 Direção predominante dos ventos: E
```

---

## 🎨 Visualizações Geradas

### 1. Precipitação Mensal Acumulada
- Gráfico de barras colorido por estação do ano
- Identificação de padrões sazonais
- Comparação com médias climatológicas

### 2. Ciclo Diário de Temperatura
- Série temporal com destaque para extremos
- Conversão automática UTC → horário local
- Análise de dinâmica térmica

### 3. Matriz de Correlações
- Heatmap completo de todas as variáveis
- Destaque para correlações significativas
- Relatório estatístico detalhado

### 4. Análise de Extremos
- Visualização de dias consecutivos secos/úmidos
- Múltiplos limiares em gráficos comparativos
- Série temporal com períodos críticos

### 5. Mapa Espacial Multi-Estações
- Grid de 12 mapas mensais
- Escala de cores consistente
- Informações estatísticas por região

---

## 🌍 Contexto Climatológico

### Bioma Cerrado - Características
- **Estação seca:** maio-setembro (120-150 dias)
- **Estação chuvosa:** outubro-abril (210-240 dias)
- **Precipitação anual típica:** 1200-1600 mm
- **Altitude de Brasília:** influencia temperaturas noturnas
- **Latitude 15°S:** resulta em variação sazonal moderada

### Avaliação Climática 2024
- **Estação seca:** 170 dias (23/04 a 09/10)
- **Classificação:** Seca EXTREMA
- **Precipitação:** 1399mm (NORMAL)
- **Padrão:** Típico do clima tropical de altitude do Cerrado

---

## 📁 Estrutura do Projeto

```
XISIC/
├── XISIC.py                    # Módulo principal
├── README.md                   # Esta documentação
├── examples/
│   ├── basic_analysis.py       # Análise básica
│   ├── correlation_analysis.py # Análise de correlações
│   ├── extreme_analysis.py     # Análise de extremos
│   └── spatial_analysis.py     # Análise espacial
├── data/
│   ├── raw/                    # Dados brutos (ZIP/CSV)
│   └── processed/              # Dados processados (NetCDF)
└── docs/
    ├── metodologia.md          # Metodologia detalhada
    └── referencias.md          # Referências bibliográficas
```

---

## 🔧 Metodologia

### Processamento de Dados
1. **Extração:** Descompactação automática de arquivos ZIP
2. **Leitura:** Tratamento robusto de diferentes encodings
3. **Validação:** Verificação de integridade dos metadados
4. **Conversão:** Estruturação para formato NetCDF CF-1.8
5. **Otimização:** Organização em dimensões multidimensionais

### Padrões Utilizados
- **CF Conventions 1.8:** Metadados padronizados
- **xclim:** Índices climatológicos validados internacionalmente
- **Pydantic:** Validação robusta de dados de entrada/saída

---

## 📈 Resultados Principais

### Correlações Significativas Encontradas (96 total)
- **Forte (|r| ≥ 0.7):** 22 correlações
- **Moderada (0.5-0.7):** 21 correlações  
- **Fraca (0.3-0.5):** 53 correlações
- **Taxa de significância:** 70.6%

### Análise de Extremos (xclim)
- **Padrão Meteorológico (≥1mm):** 170 dias secos máximos
- **Chuva Significativa (≥5mm):** 171 dias secos máximos
- **Chuva Moderada (≥10mm):** 174 dias secos máximos
- **Chuva Intensa (≥20mm):** 194 dias secos máximos

---

## 🎯 Aplicações

### Pesquisa Acadêmica
- Análises climatológicas regionais
- Estudos de variabilidade temporal
- Caracterização de eventos extremos
- Validação de modelos climáticos

### Aplicações Operacionais
- Agricultura: planejamento de cultivos
- Recursos hídricos: gestão de reservatórios
- Defesa civil: prevenção de desastres
- Energia: planejamento energético

### Preparação para COP30
- Caracterização do clima amazônico
- Análise de tendências regionais
- Subsídio para políticas climáticas
- Monitoramento de mudanças climáticas

---

## 📄 Licença

Desenvolvido para fins educacionais e científicos no contexto do XI SIC 2025.  
**Uso livre** para pesquisa acadêmica e aplicações meteorológicas.

---

## 🤝 Contribuições

Contribuições são bem-vindas! Por favor:

1. Faça um fork do projeto
2. Crie uma branch para sua feature (`git checkout -b feature/AmazingFeature`)
3. Commit suas mudanças (`git commit -m 'Add some AmazingFeature'`)
4. Push para a branch (`git push origin feature/AmazingFeature`)
5. Abra um Pull Request

---

## 📞 Contato e Suporte

- **Email:** carvalhovaldo@gmail.com
- **Evento:** [XI SIC 2025](https://sic2025.com.br/)
- **Organização:** SBMET, UFPA, UFAM

---

## 🙏 Agradecimentos

- **SBMET** - Sociedade Brasileira de Meteorologia
- **UFPA** - Universidade Federal do Pará  
- **UFAM** - Universidade Federal do Amazonas
- **INMET** - Instituto Nacional de Meteorologia (fonte dos dados)
- **Comunidade Python** - Desenvolvedores das bibliotecas utilizadas

---

**Versão:** 1.0.0  
**Criado:** Agosto 2025  
**Última atualização:** Agosto 2025

*"Promovendo o intercâmbio de conhecimentos em ciência meteorológica e preparação para discussões da COP30"*