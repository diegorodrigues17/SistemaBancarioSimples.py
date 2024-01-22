    # SistemaBancarioSimples.py
    menu = '''
    [1] Depósito
    [2] Saque
    [3] Extrato
    [4] Sair
    
    '''
    saldo = 0
    limite = 500
    extrato = ""
    numero_saques = 0
    LIMITE_SAQUE = 3
    valor = 0
    
    while True:
        opcao = (input (f"{menu} escolha uma das opções: "))    
        if opcao == "1":
            while True:
                try:    
                    valor = float(input("Escolha o valor a ser depositado: ")) 
                    break
                except ValueError:
                        print("Valor inválido!\n")
                        
            if valor > 0:
                saldo += valor
                extrato += f"\nDepósito de R${ valor:.2f}"
            

    elif opcao == "2":
        while True:
            try:
                valor = float(input("Escolha o valor do saque:"))
                break
            except ValueError:
                print("Digite um valor Válido")

            excedeu_saldo = valor > saldo
            excedeu_limite = valor > limite
            excedeu_saque = numero_saques > LIMITE_SAQUE

            if excedeu_saldo:
                print("Saldo insuficiente!")
            elif excedeu_limite:
                print("Valor indisponível para saque")
            elif excedeu_saque:
                print("Limite de saque atingido!")

            elif valor > 0:
                saldo -= valor
                extrato += f"\nSaque de R$ {valor:.2f}"
                numero_saques += 1

            else:
                print("Valor inválido")    


    elif opcao == "3":
        print("\n =============== EXTRATO ===============")
        print ("Não foram feitas alterações " if not extrato else extrato)
        print(f"\nSeu saldo é de R${saldo:.2f}")
        print ("==========================================")

    if opcao == "4":
        print("Obrigado por utilizar!")
        break
