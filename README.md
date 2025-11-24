# T4 - Diffusion Models: Outpainting & Textual Refinement

Este repositório contém uma implementação de duas tarefas de edição de imagens usando Stable Diffusion Inpainting:

1. **Outpainting**: Expansão da imagem para além dos limites originais, preenchendo áreas novas com base em um prompt textual.
2. **Textual Refinement**: Alteração controlada de uma região específica da imagem guiada por texto, preservando o restante.

Também são calculadas métricas perceptuais para comparar imagens: **SSIM** (similaridade estrutural) e **LPIPS** (distância perceptual baseada em redes profundas).

---

## Modelo Utilizado

Pipeline: `StableDiffusionInpaintPipeline` com o modelo:

```
model_id = "sd-legacy/stable-diffusion-inpainting"
```

O `safety_checker` é desativado para reduzir consumo de memória e evitar bloqueios falsos.

## Dependências

Instalação dentro do notebook:

```
pip install -q diffusers transformers accelerate torch opencv-python pillow torchmetrics lpips
```

Uso de GPU (CUDA) recomendado; o código seleciona automaticamente `cuda`, `mps` (Apple) ou `cpu`.

## Funcionalidades

### Outpainting (`perform_outpainting`)

- Cria um canvas maior (fator padrão 1.5).
- Centraliza a imagem original.
- Gera máscara onde área nova (branca) é sintetizada e área original (preta) preservada.
- Usa `strength=1.0` para reconstruir totalmente a região expandida.
- Salva resultado em `result_outpainting.png`.

### Textual Refinement (`perform_refinement`)

- Recebe imagem + máscara (região branca = área a editar).
- Redimensiona para 512x512 por consistência.
- Controla intensidade da modificação via `strength` (maior = mais liberdade criativa).
- Salva resultado em `result_refinement.png`.

### Exemplo de Máscara

Máscara simples criada cobrindo metade inferior para trocar vestimenta.

## Métricas (`calculate_metrics`)

- **SSIM**: mede similaridade estrutural (0–1, maior é melhor).
- **LPIPS (VGG)**: mede distância perceptual (0 = imagens idênticas). Tensores ajustados para faixa [-1, 1].
  Para Outpainting, recomenda-se recortar a região central antes da comparação se quiser avaliar fidelidade apenas da parte preservada.

## Arquivos Gerados

- `original.png`: imagem base (baixada da Wikimedia Commons).
- `result_outpainting.png`: imagem expandida.
- `result_refinement.png`: imagem refinada/localmente editada.

## Passos para Execução

1. Abrir `main.ipynb`.
2. Executar célula de instalação.
3. Carregar pipeline e baixar imagem.
4. Rodar células de outpainting e refinement.
5. Rodar célula de métricas.

## Extensões Possíveis

- Ajustar `expand_factor` para canvases maiores.
- Criar máscaras personalizadas (desenho manual, segmentação automática).
- Testar prompts variados para estilos (barroco, futurista, etc.).
- Adicionar métricas adicionais (FID, PSNR) se necessário.

## Licença da Imagem

Imagem usada possui status público conforme Wikimedia Commons. Verifique licenças ao trocar por outras imagens.

## Requisitos de Hardware

GPU com ≥6GB VRAM recomendada; CPU funciona porém mais lento.

---

Se precisar de script de automação ou ambiente virtual separado, posso adicionar—é só pedir.
