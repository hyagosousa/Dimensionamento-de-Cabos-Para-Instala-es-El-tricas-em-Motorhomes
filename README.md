# Dimensionamento-de-Cabos-Para-Instala-es-El-tricas-em-Motorhomes

"""
Calculadora de Dimensionamento de Cabos para Instalações Elétricas em Motohomes.
Suporta sistemas de Corrente Contínua (12V/24V) e Corrente Alternada (127V/220V).
Baseado na norma NBR 5410 e critérios de queda de tensão máxima permitida.
"""

# Tabela simplificada de resistividade e capacidade aproximada para cobre (mm²)
# Formato: Bitola (mm²): {"resistencia_cc": ohm/km, "capacidade": Amperes}
TABELA_CABOS_CC = {
    1.5: {"resistencia": 12.1, "capacidade": 15},
    2.5: {"resistencia": 7.41, "capacidade": 21},
    4.0: {"resistencia": 4.61, "capacidade": 28},
    6.0: {"resistencia": 3.08, "capacidade": 36},
    10.0: {"resistencia": 1.83, "capacidade": 50},
    16.0: {"resistencia": 1.15, "capacidade": 68},
    25.0: {"resistencia": 0.727, "capacidade": 89},
    35.0: {"resistencia": 0.524, "capacidade": 111},
    50.0: {"resistencia": 0.368, "capacidade": 134},
}


def calcular_cabo(corrente_a, distancia_m, tensao_v, tipo_corrente="CC", queda_max_pct=3.0):
    """
    Calcula a bitola mínima do cabo com base na queda de tensão máxima permitida.
    
    Parâmetros:
    - corrente_a: Corrente total do circuito em Amperes (A).
    - distancia_m: Distância em metros (APENAS UM SENTIDO: da bateria/fonte até o equipamento).
    - tensao_v: Tensão do sistema em Volts (ex: 12, 24, 127, 220).
    - tipo_corrente: "CC" (Corrente Contínua - padrão 12V) ou "CA" (Corrente Alternada).
    - queda_max_pct: Porcentagem máxima de queda de tensão aceitável (Recomendado: 3% a 5%).
    """
    
    # Em CC, a corrente percorre o positivo e retorna pelo negativo (ida e volta = 2x)
    fator_distancia = 2 if tipo_corrente == "CC" else 1
    
    # Queda de tensão máxima em Volts
    delta_v_max = tensao_v * (queda_max_pct / 100.0)
    
    # Fórmula da seção mínima (S) baseada na queda de tensão:
    # S = (2 * rho * L * I) / Delta_V
    # Onde rho do cobre mole é aproximadamente 0.0172 (ohm*mm²/m)
    rho = 0.0172
    secao_calculada = (fator_distancia * rho * distancia_m * corrente_a) / delta_v_max
    
    # Encontrar a primeira bitola comercial que atende à queda E à capacidade de corrente
    bitola_selecionada = None
    capacidade_suportada = 0
    
    for bitola, dados in sorted(TABELA_CABOS_CC.items()):
        if bitola >= secao_calculada and dados["capacidade"] >= corrente_a:
            bitola_selecionada = bitola
            capacidade_suportada = dados["capacidade"]
            break
            
    # Se a corrente for muito alta para a tabela básica
    if not bitola_selecionada:
        maior_bitola = max(TABELA_CABOS_CC.keys())
        bitola_selecionada = maior_bitola
        capacidade_suportada = TABELA_CABOS_CC[maior_bitola]["capacidade"]

    return {
        "secao_minima_teorica": round(secao_calculada, 2),
        "bitola_comercial_recomendada_mm2": bitola_selecionada,
        "capacidade_maxima_cabo_a": capacidade_suportada,
        "queda_max_permitida_v": round(delta_v_max, 2)
    }


if __name__ == "__main__":
    print("==================================================")
    print("   CALCULADORA DE CABOS PARA MOTOHOME (PYTHON)    ")
    print("==================================================")
    
    # Exemplo prático: Bomba d'água ou Geladeira 12V
    # Corrente: 10A, Distância: 4 metros (da bateria até a geladeira), Tensão: 12V
    I = 10.0  # Amperes
    L = 4.0   # Metros (ida)
    V = 12.0  # Volts
    
    resultado = calcular_cabo(corrente_a=I, distancia_m=L, tensao_v=V, tipo_corrente="CC", queda_max_pct=3.0)
    
    print(f"\n[Exemplo Prático] Equipamento 12V / 10A / Distância: 4m")
    print(f"-> Seção Teórica Mínima: {resultado['secao_minima_teorica']} mm²")
    print(f"-> Bitola Comercial Recomendada: **{resultado['bitola_comercial_recomendada_mm2']} mm²**")
    print(f"-> Capacidade do Cabo Escolhido: {resultado['capacidade_maxima_cabo_a']} A")
