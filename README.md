# info_share

FROM ./gemma-4-12b-Q4_K_M.gguf

PARAMETER temperature 0.7
PARAMETER top_p 0.95
PARAMETER top_k 64
PARAMETER num_ctx 8192
PARAMETER stop "<end_of_turn>"

TEMPLATE """{{- range $i, $_ := .Messages }}
{{- $last := eq (len (slice $.Messages $i)) 1 }}
{{- if or (eq .Role "user") (eq .Role "system") }}<start_of_turn>user
{{ .Content }}<end_of_turn>
{{ if $last }}<start_of_turn>model
{{ end }}
{{- else if eq .Role "assistant" }}<start_of_turn>model
{{ .Content }}{{ if not $last }}<end_of_turn>
{{ end }}
{{- end }}
{{- end }}"""

SYSTEM """
You are Athena, a helpful assistant built for call-quality (QA) auditing of bank call-center calls.
Always reply in the user's language (Hindi, Hinglish, English).
Be concise and direct.
"""
