biblioteca = []

nomes_iniciais = ["Dom Casmurro","O Alquimista","A Hora da Estrela","Cem Anos de Solidão","1984"]
nomes_autores = ["Machado de Assis","Paulo Coelho","Clarice Lispector","Gabriel García Márquez","George Orwell"] 
nomes_lançamento = ["1899","1988","1977","1967","1949"]

def carregar_nomes_iniciais():
    for t, a, l in zip(nomes_iniciais, nomes_autores, nomes_lançamento):
        biblioteca.append({"titulo": t, "autor": a, "ano": l, "status": "disponível"})

def safe_input(prompt):
    try:
        return input(prompt)
    except (EOFError, KeyboardInterrupt):
        print("\nEntrada interrompida. Use opção 5 para sair.")
        return ""

def cadastrar_livro():
    print("\n=== Cadastro ===")
    while True:
        titulo = safe_input("Título: ").strip()
        if titulo: break
        print("Título vazio. Tente novamente.")
    biblioteca.append({"titulo": titulo, "autor": safe_input("Autor: ").strip(), "ano": safe_input("Ano: ").strip(), "status": "disponível"})
    print("Livro cadastrado.\n")

def listar_livros(apenas_titulos=False):
    if not biblioteca:
        print("\nNenhum livro cadastrado.\n"); return
    print("\n=== Lista ===")
    for i, l in enumerate(biblioteca, 1):
        print(f"{i}. {l['titulo']}" if apenas_titulos else f"{i}. {l['titulo']} - {l['autor']} ({l['ano']}) [{l['status']}]")
    print(f"\nTotal: {len(biblioteca)}\n")

def buscar_titulo():
    termo = safe_input("\nTítulo (ou parte): ").strip().lower()
    if not termo:
        print("Busca vazia.\n"); return
    encontrados = [l for l in biblioteca if termo in l["titulo"].lower()]
    if encontrados:
        for l in encontrados: print(f"- {l['titulo']} - {l['autor']} ({l['ano']}) [{l['status']}]")
        print()
    else:
        print("Nenhum resultado.\n")

def alterar_status():
    titulo = safe_input("\nTítulo exato: ").strip().lower()
    if not titulo:
        print("Título não informado.\n"); return
    for l in biblioteca:
        if l["titulo"].lower() == titulo:
            l["status"] = "emprestado" if l["status"] == "disponível" else "disponível"
            print(f"Status atualizado: {l['titulo']} -> {l['status']}\n")
            return
    print("Livro não encontrado.\n")

def menu():
    ops = {"1": cadastrar_livro, "2": listar_livros, "3": buscar_titulo, "4": alterar_status, "6": lambda: listar_livros(True)}
    while True:
        print("=== MENU ===\n1.Cadastrar livro  2.Listar livros  3.Buscar por titulos  4.Marcar livros  5.Sair  6.Apenas títulos")
        opc = safe_input("Escolha: ").strip()
        if opc == "5":
            print("Saindo...\n"); break
        action = ops.get(opc)
        if action:
            action()
        else:
            print("Opção inválida.\n")

carregar_nomes_iniciais()
menu()
