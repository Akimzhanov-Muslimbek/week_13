# Week 13 — Raydium CLMM Position

Это отдельная копия проекта для недели 13. В ней сохранены предыдущие инструкции Vault и Raydium Swap, потому что Week 13 использует их как основу, и добавлены:

- `open_position` — размещение TOKEN_A и TOKEN_B в диапазоне цен Raydium CLMM;
- `collect_fees` — сбор комиссий позиции;
- `close_position` — закрытие позиции и возврат ликвидности в Vault;
- проверки `PositionAlreadyOpen`, `NoOpenPosition` и `PositionOpen`;
- учёт NFT позиции, принадлежащего PDA Vault.

## Важно перед запуском

- Этот архив не содержит приватного ключа программы, кошелька, `target` или `node_modules`.
- Не используйте Program ID Week 12. Для этой отдельной программы создайте новый Program ID.
- В `Anchor.toml` и `lib.rs` временно стоит системный placeholder ID `11111111111111111111111111111111`. Его нужно заменить на ID новой пары ключей командой `anchor keys sync` после первой сборки.
- Работать нужно в Devnet. Тесты Raydium создают настоящие токены, пулы, tick arrays и позицию; они расходуют SOL, а созданные on-chain аккаунты остаются в Devnet.
- Week 13 меняет структуру `VaultState`. Разворачивайте как отдельную программу и создавайте новый Vault. Не направляйте этот код на существующие аккаунты Week 12.

## Установка и сборка (WSL Ubuntu)

Из каталога `vault-week13`:

```bash
yarn install
anchor build
anchor keys list
anchor keys sync
anchor build
anchor keys list
```

После `anchor keys sync` адрес `simple_vault` в `Anchor.toml` и `declare_id!` в `programs/simple_vault/src/lib.rs` должен совпадать с результатом `anchor keys list`.

Проверьте настройки кошелька и сети:

```bash
solana config get
solana address
solana balance
```

Убедитесь, что выбран Devnet и правильный кошелёк. Затем разверните отдельную программу:

```bash
anchor deploy --provider.cluster devnet
solana program show <ВАШ_PROGRAM_ID>
```

## Тестирование

Задайте переменные окружения в текущем терминале:

```bash
export ANCHOR_PROVIDER_URL=https://api.devnet.solana.com
export ANCHOR_WALLET=$HOME/.config/solana/id.json
```

Базовые тесты Vault:

```bash
yarn test-vault
```

Тесты Raydium (включают Week 12 и Week 13 сценарии):

```bash
yarn test-raydium
```

Полный набор:

```bash
yarn test
```

Не запускайте Raydium-тесты повторно без необходимости: они каждый раз создают новые пулы и аккаунты в Devnet и могут расходовать заметное количество SOL.

## Что проверяют тесты Week 13

- вызов `open_position` не администратором отклоняется;
- пул с неправильной парой отклоняется;
- Vault открывает CLMM-позицию в диапазоне цен;
- повторное открытие позиции запрещено;
- `collect_fees` и `close_position` защищены проверкой администратора;
- нельзя собирать комиссии с чужой позиции;
- комиссии поступают в Vault;
- при закрытии позиции ликвидность возвращается в Vault;
- `sync_vault` запрещён, пока позиция открыта;
- после закрытия позиции Vault можно вернуть в обычный режим.

## Состав проекта

- `programs/simple_vault/src/lib.rs` — Anchor-программа и инструкции Vault;
- `programs/simple_vault/src/raydium.rs` — CPI-интеграция с Raydium CLMM;
- `tests/vault.ts` — тесты базового Vault;
- `tests/raydium.ts` — тесты Swap и CLMM Position;
- `scripts/create-clmm-pool.ts` — вспомогательный скрипт создания CLMM-пула;
- `README.md` — расширенное описание проекта.
