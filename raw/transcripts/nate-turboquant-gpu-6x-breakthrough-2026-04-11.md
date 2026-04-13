# TurboQuant — GPU Memory Breakthrough

## Source Metadata
- **Title:** GPUs Just Got 6x More Valuable. No New Hardware Required.
- **Author:** Nate (Nate's Substack)
- **Published:** April 11, 2026
- **URL:** (Substack transcript)
- **Type:** Substack article + video transcript
- **Date captured:** 2026-04-12

## Transcript Summary
Google Research published TurboQuant on March 25, 2026. It compresses the KV cache (LLM working memory) by 6x with zero accuracy loss. No retraining, no calibration, no new hardware. The same GPU that served 9 concurrent users now serves 50+.

Key technical mechanism:
1. **PolarQuant** — rotates data into polar coordinates where structure becomes predictable; eliminates per-block normalization overhead
2. **QJL** (Quantized Johnson-Lindenstrauss) — corrects residual error with single-bit check

Result: KV cache compressed from 16/32 bits per value down to 3 bits, with no measurable accuracy loss across QA, code generation, summarization, and needle-in-a-haystack retrieval at 104K tokens.

Related research cited:
- Percepta: compiled WebAssembly interpreter into transformer weights; model executes arbitrary C programs through its forward pass
- ShadowKV, KIVI, FlashAttention v1-v3 as prior KV cache compression work
- H2O (Heavy-Hitter Oracle), StreamingLLM for eviction strategies
- DeepSeek-V2's MLA (Multi-Head Latent Attention)
- vLLM FP8 KV cache (production-ready today)

Neuroscience connection: Frankland's lab (U of Toronto) and Nottingham MEG study show human memory is a retrieval deficit not a storage deficit; brain stores "memory keys" that can reactivate "forgotten" memories — structurally analogous to PolarQuant's radius+angles representation.

Competitive dynamics:
- Google wins twice: wrote TurboQuant + runs Gemini
- NVIDIA's hardware sales urgency potentially reduced
- Middleware squeezed (layer collapse)
- Enterprises running self-hosted inference get immediate relief via vLLM FP8 or community TurboQuant implementations

Three-body framing of AI memory crisis:
1. Supply constrained (fab timelines, years)
2. Demand exploding (agent adoption, quarters)
3. Compression advancing (speed of math papers, weeks)

Article also mentions Open Brain (author's personal knowledge infrastructure product).
