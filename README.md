# Cadeira de rodas controlada por voz

[Português](README.md) | [English](README.en.md)

Protótipo de machine learning que reconhece comandos de movimento falados e os utiliza para controlar uma cadeira de rodas numa simulação interativa. O projeto cobre o pipeline completo de classificação de áudio: exploração do sinal, engenharia de features, seleção de modelos, avaliação e demonstração em tempo real com microfone.

> Esta é uma simulação académica de software. Não está certificada nem é adequada para controlar um dispositivo de mobilidade real.

## Comandos suportados

| Classe de áudio | Ação na simulação |
|---|---|
| `forward` | Avançar |
| `backward` | Recuar |
| `left` | Virar à esquerda |
| `right` | Virar à direita |
| `stop` | Parar |
| `_silence_` | Ignorar silêncio/ruído de fundo |
| `_unknown_` | Rejeitar palavras não suportadas |

## Pipeline

```mermaid
flowchart LR
    A[Gravações WAV] --> B[Limpeza e normalização]
    B --> C[Extração de features de áudio]
    C --> D[Seleção de features]
    D --> E[Avaliação KNN e MLP]
    E --> F[Modelo serializado]
    F --> G[Inferência pelo microfone]
    G --> H[Simulação Pygame]
```

O notebook implementa:

1. inspeção do dataset, distribuição das classes e normalização dos WAV;
2. análise de duração/amplitude e deteção de outliers com Z-score, IQR, K-Means e DBSCAN;
3. features temporais, espectrais, STFT, MFCC e wavelets;
4. testes estatísticos e seleção com PCA, Fisher Score e ReliefF;
5. avaliação train/test, train/validation/test e stratified K-fold;
6. comparação de hiperparâmetros para KNN e MLP do scikit-learn;
7. rede neuronal e backpropagation implementadas de raiz para aprendizagem;
8. inferência do microfone em tempo real num labirinto Pygame.

## Tecnologias

- Python e Jupyter Notebook;
- NumPy, pandas e SciPy;
- librosa e PyWavelets para processamento de sinal;
- scikit-learn para preparação, seleção de features e classificação;
- Matplotlib para exploração e avaliação;
- sounddevice para captura do microfone;
- Pygame para a simulação interativa.

## Estrutura

```text
Voice_controlled_Wheelchair/
├── project.ipynb      # análise, treino, avaliação e simulação
├── requirements.txt   # dependências Python
├── README.md
└── README.en.md
```

O dataset de áudio e os modelos gerados não estão incluídos no repositório.

## Executar localmente

```bash
git clone https://github.com/josepedrocunhazzz/voice_controlled_wheelchair.git
cd voice_controlled_wheelchair
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

No Windows, ativar com `.venv\Scripts\activate`. O `sounddevice` pode exigir também a instalação de PortAudio no sistema operativo.

Colocar os ficheiros WAV num diretório por classe:

```text
dataset/
├── forward/
├── backward/
├── left/
├── right/
├── stop/
├── _silence_/
└── _unknown_/
```

O notebook mantém o `dataset_path` absoluto do ambiente original em várias células. Substituir cada atribuição pelo caminho do dataset local e executar:

```bash
jupyter lab project.ipynb
```

O treino cria `mlp_combined_model.pkl` e `feature_statistics.pkl`, usados pela demo em tempo real. A simulação final requer sessão gráfica, permissão para o microfone e um dispositivo de entrada funcional.

## Notas de avaliação

As experiências comparam F1 ponderado, accuracy, precision, recall e matrizes de confusão. KNN e o MLP do scikit-learn atingiram desempenho moderado. Um resultado bruto superior na rede implementada de raiz foi afetado pelo desbalanceamento e por previsões concentradas em poucas classes, pelo que não deve ser interpretado como o melhor modelo.

Num cenário de controlo assistivo, a accuracy agregada pode esconder erros perigosos em comandos menos frequentes. Recall por classe, matrizes de confusão, latência e rejeição robusta de silêncio/palavras desconhecidas são medidas mais importantes para trabalho futuro.

## Contexto académico

Projeto universitário que demonstra um workflow de ciência de dados, processamento digital de sinal e machine learning. A utilização em hardware físico exigiria restrições de segurança em tempo real, mecanismos fail-safe, testes extensivos com utilizadores e validação por especialistas em tecnologia assistiva.
