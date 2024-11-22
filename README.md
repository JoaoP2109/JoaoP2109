#Perguntas iniciais para análise
def coletar_informacoes(produto_num):
    print("-------------------------------------------------------------------------------")
    print(f"|                 Controle de Estoque Inteligente - Produto {produto_num + 1}                 |")
    print(f"-------------------------------------------------------------------------------")

    vendas_do_mes = int(input("Quantos itens desse produto foram vendidios no último mês? "))
    estoque_atual = int(input("Quantas unidades estão disponíveis no estoque atualmente? "))
    nivel_minimo = int(input("Qual é a quantidade mínima de unidades que deve ser mantida no estoque? "))

    # sazonalidade = Aumento de demanda em períodos específicos, ou seja, a procura por esses produtos.
    sazonalidade = input("Há aumento de demanda sazonal esperado (sim/não)? ").strip().lower()
    promocao = input("Há alguma promoção planejada que pode impactar as vendas (sim/não)? ").strip().lower()


    prazo_fornecedor = int(input("Quantos dias leva para repor este produto com o fornecedor? "))
    atraso_fornecedor = input("O fornecedor costuma atrasar (sim/não)? ").strip().lower()

    validade = input("O produto tem validade (sim/não)? ").strip().lower()
    if validade == "sim":
        dias_validade = int(input("Quantos dias dura o seu produto? "))
    else:
        dias_validade = "não"

    return {
        "vendas_do_mes": vendas_do_mes,
        "estoque_atual": estoque_atual,
        "nivel_minimo": nivel_minimo,
        "sazonalidade": sazonalidade,
        "promocao": promocao,
        "prazo_fornecedor": prazo_fornecedor,
        "atraso_fornecedor": atraso_fornecedor,
        "validade": validade,
        "dias_validade": dias_validade,
    }

#Esta função calcula a quantidade estimada de produtos para a reposição com base nos dados fornecidos.
def calcular_reposicao(dados, produto_num):
    print("\n-------------------------------------------------------------------------------")
    print(f"|                      Análise de Reposição - Produto {produto_num + 1}                       |")
    print("-------------------------------------------------------------------------------")

    vendas_do_mes = dados["vendas_do_mes"]
    estoque_atual = dados["estoque_atual"]
    nivel_minimo = dados["nivel_minimo"]
    sazonalidade = dados["sazonalidade"]
    promocao = dados["promocao"]
    prazo_fornecedor = dados["prazo_fornecedor"]
    atraso_fornecedor = dados["atraso_fornecedor"]
    validade = dados["validade"]
    dias_validade = dados["dias_validade"]

    if validade == "sim" and dias_validade != "não" and dias_validade < 30:
        print("                                         🤢                                           ")
        print("\033[1mAtenção: seus produtos podem estar fora da validade.\033[0m")
        quantidade_repor = max(nivel_minimo * 2 - estoque_atual, vendas_do_mes)
        print(f"Sugestão: Repor {quantidade_repor} unidades para evitar riscos à saúde.\n\n")

    elif estoque_atual < nivel_minimo:
        print("                                         ⚠️                                           ")
        print("\033[1mEstoque abaixo do nível mínimo! Reposição imediata recomendada.\033[0m")
        quantidade_repor = max(nivel_minimo * 2 - estoque_atual, vendas_do_mes)
        print(f"Sugestão: Repor {quantidade_repor} unidades.\n\n")

    elif sazonalidade == "sim" or promocao == "sim":
        print("                                         📈                                           ")
        print("\033[1mDemanda aumentada esperada devido a sazonalidade ou promoção.\033[0m")
        quantidade_repor = int(vendas_do_mes * 1.5)  # Reposição com folga
        print(f"Sugestão: Repor {quantidade_repor} unidades para atender à demanda extra.\n\n")

    elif prazo_fornecedor > 7 or atraso_fornecedor == "sim":
        print("                                         🚚                                           ")
        print("\033[1mPrazo longo ou atraso frequente do fornecedor detectado.\033[0m")
        quantidade_repor = int(vendas_do_mes * 1.2)
        print(f"Sugestão: Repor {quantidade_repor} unidades para evitar rupturas.\n\n")

    else:
        print("✅ Estoque e demanda sob controle. Não é necessária reposição imediata.\n\n")

#Função principal. Esta função definirá o número de produtos disponiveis na loja, coletará informaçõs e fará a análise final.
def main():

    num_produtos = int(input("Quantos produtos há na loja? "))

    for i in range(num_produtos):
        dados = coletar_informacoes(i)
        calcular_reposicao(dados, i)


if __name__ == "__main__":
    main()
