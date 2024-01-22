def menu ():
    menu = """"
[1] Deposito
[2] Saque
[3] Extrato
[4] Nova conta
[5] Listar contas
[6] Novo usuário
[7] Sair
"""
    return input(menu)

def depositar (saldo,valor,extrato,/):

    if valor > 0:
        saldo += valor
        extrato += f"\nDeposito de R${ valor:.2f}"
        print("Valor depositado")
    else:
      print("valor inválido")
    return saldo,extrato

def sacar(*,saldo,valor,extrato,limite,numero_saques,LIMITE_SAQUE):
    excedeu_saldo = valor > saldo
    excedeu_limite = valor > limite
    excedeu_saque = numero_saques > LIMITE_SAQUE

    if excedeu_saldo:
        print("Saldo insuficiente!")
    elif excedeu_limite:
        print("Valor indisponivel para saque")
    elif excedeu_saque:
        print("Limite de saque atingido!")

    elif valor > 0:
        saldo -= valor
        extrato += f"\nSaque de R$ {valor:.2f}"
        numero_saques += 1
        print("Saque realizado")

    else:
      print("Valor invalido")

    return saldo,extrato

def exibir_extrato (saldo,/,*,extrato):
    print("\n =============== EXTRATO ===============")
    print ("Não foram feitas alterações " if not extrato else extrato)
    print(f"\nSeu saldo é de R${saldo:.2f}")
    print ("==========================================")

def criar_ususarios(usuarios):
    cpf= input("Digite seu CPF(somente núemros)")
    usuario = filtar_usuario(cpf,usuarios)

    if usuario:
        print("Já existe usuário com esse CPF!")
        return
    nome = input("Informe seu nome completo:  ")
    data_nascimento = input("Informe sua data de nascimento (dd-mm-aaaa): ")
    endereco = input("Informe seu endereço (logradouro, número-bairo-cidade/sigla estado: )")
    usuarios.append({"nome":nome, "data_nascimento":data_nascimento,"cpf":cpf, "endereco":endereco})
    print("Usuário criado com sucesso!")

def filtar_usuario(cpf,usuarios):
    usuarios_filtrados = [usuario for usuario in usuarios if usuario["cpf"] == cpf ]
    return usuarios_filtrados [0] if usuarios_filtrados else None

def criar_conta(agencia,numero_conta,usuarios):
    cpf = input("Informe o CPF do usuário: ")
    usuario = filtar_usuario(cpf,usuarios)

    if usuario:
        print("Conta Criada com sucesso!")
        return {"agencia":agencia,"numero_conta":numero_conta,"usuario":usuario}
    print("Usuario não encontrado")

def listar_contas(contas):
    for conta in contas:
        linha = f"""/ Agência {conta['agencia']}
           C/C:  {conta['numero_conta']}
            Titular: {conta['usuario']['nome']}
        """
        print("=" * 100)
        print(linha)

def main():
    saldo = 0
    limite = 500
    extrato = ""
    numero_saques = 0
    LIMITE_SAQUE = 3
    AGENCIA ="0001"
    valor = 0
    usuario =[]
    contas = []
    while True:
        opcao = menu()
        if opcao == "1":
            valor = float(input("Escolha o valor a ser depositado: "))

            saldo, extrato = depositar(saldo,valor,extrato)

        elif opcao == "2":
            valor = float(input("Escolha o valor do saque:"))

            saldo, extrato = sacar(
                saldo = saldo,
                valor = valor,
                extrato = extrato,
                limite = limite,
                numero_saques = numero_saques,
                LIMITE_SAQUE = LIMITE_SAQUE,
            )

        elif opcao == "3":
          exibir_extrato(saldo, extrato=extrato)

        if opcao == "4":
            numero_conta = len(contas) + 1
            conta = criar_conta(AGENCIA,numero_conta,usuario)

            if conta:
                contas.append(conta)

        elif opcao == "5":
            listar_contas(contas)

        elif opcao == "6":
            criar_ususarios(usuario)

        elif opcao == "7":
            break

        elif opcao not in ("1,2,3,4,5,6,7"):
            print("Operação inválida")

main()
