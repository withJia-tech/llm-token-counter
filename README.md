# llm-token-counter
Situation - context windows are 200k usually (of course they keep expanding ), but we have the 160k threshold before we enter into 'dumbzone'. 
Complication - different LLMs have different rates of consuming tokens 
Question - then how can we help users build that muscle discipline (for now until the 160k tokens threshold is lifted ) to check and be more efficient with their prompts and context? 
Answer: to create a simple artifact that allows foolproof measurement of different LLMs ingesting the same context and spit out the tokens consumed so that user is aware

:)

So before I count it manually, this was the landscape presented:
- OpenAI (GPT-4, GPT-4o, GPT-3.5) — fully open. Their tokenizer (tiktoken) is published. You can run it in browser, Python, or Node and get exact counts.
- Anthropic (Claude) — the tokenizer is not publicly published. Anthropic's docs give a rule of thumb (~3.5 characters per token for English). The API has a count_tokens endpoint if you have a key, but there's no offline tokenizer for Claude. So Claude counts are always estimates unless you call the API.
- Google (Gemini) — has a countTokens API endpoint, also requires a key. Open weights variants (Gemma) have public tokenizers.
Open models (Llama, Mistral, Qwen) — fully open tokenizers via Hugging Face.

Good news? Browser-based options that exist already:
1. tiktokenizer.vercel.app — popular, supports OpenAI tokenizers
2. Anthropic's docs — has API examples

What's missing: a single, self-contained, offline tool that shows me all 4 side-by-side AND tells me what fraction of each LLM's context window you'd consume. So let me build it.

How it actually works under the hood?
1. Loads gpt-tokenizer from esm.sh (an ESM CDN) — this gives you the real OpenAI tokenizer, not an approximation
2. Computes Claude estimate as chars / 3.5 (Anthropic's published rule of thumb)
3. Computes Llama estimate as chars / 3.8
4. All in-memory, no localStorage, no API keys, no backend (thank goodness - I cannot imagine API $$)

What this helps with?
For users: paste anything before sending it to me — a long article draft, a corpus of research papers, a code dump — and you'll see immediately whether you're about to nuke the context window. Especially useful when you want to drop Microsoft's full MSRC post or a 30-page paper into a conversation.
For GPT: indirectly. If you can self-monitor before sending big inputs, with the GPT's help collectively avoid the dumb zone. You become GPT's context-budget partner.
>.<
