
# go-bip39x (fork of tyler-smith/go-bip39 v1.1.0)

**Что изменено**
- ✅ Потокобезопасные мультиязычные API (без глобальных `SetWordList`): `DetectLanguage`, `EntropyFromMnemonicLang`, `EntropyFromMnemonicAuto`, `IsMnemonicValidAny`.
- ✅ Быстрый путь **English → Auto-fallback**: `IsMnemonicValidPreferEnglish`, `EntropyFromMnemonicPreferEnglish`.
- ✅ `NewSeed` теперь делает **NFKD-нормализацию** мнемоники и пароля (требование BIP‑39).
- ✅ Устойчивость к различным пробелам (включая японский U+3000), схлопывание пробелов.
- ↩️ Старые функции оставлены (совместимость).

## Как подключить без правки импортов
В вашем проекте оставить:
```go
import "github.com/tyler-smith/go-bip39"
```
и добавить в `go.mod`:
```
replace github.com/tyler-smith/go-bip39 => github.com/ProjectxFi/go-bip39 v1.1.1
```

## Новый API (пример)
```go
// Быстрая проверка с приоритетом английского
ok, lang := bip39.IsMnemonicValidPreferEnglish(mnemonic)

// Автодетект + энтропия
ent, lang, err := bip39.EntropyFromMnemonicAuto(mnemonic)

// Явный язык
ent, err := bip39.EntropyFromMnemonicLang(mnemonic, bip39.Japanese)

// Seed с корректной NFKD-нормализацией
seed := bip39.NewSeed(mnemonic, passphrase)
```

## Для вашего модуля
Замените:
```go
if !bip39.IsMnemonicValid(row) { ... }
```
на:
```go
valid, lang := bip39.IsMnemonicValidPreferEnglish(row)
if !valid { ... }
_ = lang // можно логировать
```
`hdwallet.NewFromMnemonic(row)` работать будет как раньше, но теперь корректно для любых языков.
