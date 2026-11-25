# Начало работы — Week 13

1. Откройте эту папку в WSL Ubuntu.
2. Выполните `yarn install`.
3. Выполните `anchor build`.
4. Выполните `anchor keys sync`, затем снова `anchor build`.
5. Убедитесь, что ID из `anchor keys list`, `Anchor.toml` и `declare_id!` совпадают.
6. Проверьте Devnet и кошелёк; затем `anchor deploy --provider.cluster devnet`.
7. Следуйте инструкциям из `WEEK13_RU.md` для запуска тестов.

Не загружайте в GitHub `target/`, `node_modules/`, `id.json`, `*keypair*.json` или `.env`.
