// Exercícios de TypeScript
// Matéria: Aplicação Móveis - Profº Brendo Vale
// Integrante da Atividade: Ana Julia Batista do Rosário

import { useState } from "react";
import { View, Text, TouchableOpacity, StyleSheet } from "react-native";

// ==========================
// Exercício 1: Calcular IMC
// ==========================
interface calcularIMCProps {
    peso: number;
    altura: number;
}
function calcularIMC({peso, altura}: calcularIMCProps): number {
    const IMC = peso / (altura * altura);
    return IMC;
}

// =====================================
// Exercício 2: Formatar nome completo
// =====================================
interface formatarNomeProps {
    nome: string;
    sobrenome?: string; // opcional
}
function formatarNome({nome, sobrenome}: formatarNomeProps): string {
    return sobrenome ? `${nome} ${sobrenome}` : nome;
}

// ===================================
// Exercício 3: Verificar maioridade
// ===================================
interface verificarMaioridadeProps {
    idade: number;
}
function verificarMaioridade({idade}: verificarMaioridadeProps): boolean {
    return idade >= 18;
}

// ==================================
// Exercício 4: Interface de Produto
// ==================================
interface Produto {
    id: number;
    nome: string;
    preco: number;
    descricao?: string; // opcional
}
function formatarProduto({id, nome, preco, descricao}: Produto): string {
    return descricao
        ? `Produto: ${nome} ID: ${id} Preço: R$ ${preco.toFixed(2)} Descrição: ${descricao}`
        : `Produto: ${nome} ID: ${id} Preço: R$ ${preco.toFixed(2)}`;
}

// ===================================
// Exercício 5: Filtrar números pares
// ===================================
interface filtrarParesProps {
    numeros: number[];
}
function filtrarPares({numeros}: filtrarParesProps): number[] {
    return numeros.filter(num => num % 2 === 0);
}

// ===================================
// Exercício 6: Converter temperatura
// ===================================
type UnidadeTemperatura = "celsius" | "fahrenheit";
interface converterTemperaturaProps {
    valor: number;
    unidade: UnidadeTemperatura;
}
function converterTemperatura({valor, unidade}: converterTemperaturaProps): number {
    if (unidade === "celsius") {
        return (valor * 9 / 5) + 32;
    } else {
        return (valor - 32) * 5 / 9;
    }
}

// ===========================================
// Exercício 7: Contar Ocorrências (Genérico)
// ===========================================
interface contarOcorrenciasProps<T> {
    array: T[];
    elemento: T;
}
function contarOcorrencias<T>({array, elemento}: contarOcorrenciasProps<T>): number {
    return array.filter(item => item === elemento).length;
}

// ================================
// Exercício 8: Interface de Aluno
// ================================
interface Aluno {
    nome: string;
    notas: number[];
    matricula: string;
}
function calcularMedia(aluno: Aluno): number {
    if (aluno.notas.length === 0) return 0;
    const soma = aluno.notas.reduce((acc, nota) => acc + nota, 0);
    return soma / aluno.notas.length;
}

// ================================================
// Exercício 9: Tipo de Resposta de API (Genérico)
// ================================================
type ApiResponse<T> = {
    sucesso: boolean;
    dados: T | null;
    erro: string | null;
};
interface Usuario {
    id: number;
    nome: string;
    email: string;
}
function buscarUsuarios(): ApiResponse<Usuario[]> {
    const usuarios: Usuario[] = [
        { id: 1, nome: "Ana Julia", email: "ana@email.com" },
        { id: 2, nome: "Brendo Vale", email: "brendo@email.com" },
        { id: 3, nome: "Ricardo Santos", email: "Ricardo@email.com" },
    ];
    return {
        sucesso: true,
        dados: usuarios,
        erro: null,
    };
}

// ====================================================
// Exercício 10: Componente Tipado — Lista de Tarefas
// ====================================================
interface Tarefa {
    id: number;
    titulo: string;
    concluida: boolean;
}
interface ListaTarefasProps {
    tarefas: Tarefa[];
    onToggle: (id: number) => void;
}
export default function ListaTarefas({ tarefas, onToggle }: ListaTarefasProps) {
    const [filtro, setFiltro] = useState<"todas" | "pendentes" | "concluidas">("todas");

    const tarefasFiltradas: Tarefa[] = tarefas.filter((tarefa) => {
        if (filtro === "pendentes") return !tarefa.concluida;
        if (filtro === "concluidas") return tarefa.concluida;
        return true;
    });

    return (
        <View style={styles.container}>

            <View style={styles.filtros}>
                <TouchableOpacity
                    style={[styles.botao, filtro === "todas" && styles.botaoAtivo]}
                    onPress={() => setFiltro("todas")}
                >
                    <Text style={styles.botaoTexto}>Todas</Text>
                </TouchableOpacity>

                <TouchableOpacity
                    style={[styles.botao, filtro === "pendentes" && styles.botaoAtivo]}
                    onPress={() => setFiltro("pendentes")}
                >
                    <Text style={styles.botaoTexto}>Pendentes</Text>
                </TouchableOpacity>

                <TouchableOpacity
                    style={[styles.botao, filtro === "concluidas" && styles.botaoAtivo]}
                    onPress={() => setFiltro("concluidas")}
                >
                    <Text style={styles.botaoTexto}>Concluídas</Text>
                </TouchableOpacity>
            </View>

            {tarefasFiltradas.map((tarefa) => (
                <TouchableOpacity
                    key={tarefa.id}
                    style={styles.tarefa}
                    onPress={() => onToggle(tarefa.id)}
                >
                    <Text style={[styles.tarefaTexto, tarefa.concluida && styles.tarefaConcluida]}>
                        {tarefa.concluida ? "✅" : "⬜"} {tarefa.titulo}
                    </Text>
                </TouchableOpacity>
            ))}

            {tarefasFiltradas.length === 0 && (
                <Text style={styles.vazio}>Nenhuma tarefa encontrada.</Text>
            )}

        </View>
    );
}

const styles = StyleSheet.create({
    container: {
        flex: 1,
        padding: 16,
    },
    filtros: {
        flexDirection: "row",
        justifyContent: "space-around",
        marginBottom: 16,
    },
    botao: {
        paddingVertical: 8,
        paddingHorizontal: 16,
        borderRadius: 8,
        backgroundColor: "#e0e0e0",
    },
    botaoAtivo: {
        backgroundColor: "#4A90E2",
    },
    botaoTexto: {
        color: "#333",
        fontWeight: "bold",
    },
    tarefa: {
        padding: 12,
        marginBottom: 8,
        backgroundColor: "#f9f9f9",
        borderRadius: 8,
        borderWidth: 1,
        borderColor: "#ddd",
    },
    tarefaTexto: {
        fontSize: 16,
        color: "#333",
    },
    tarefaConcluida: {
        textDecorationLine: "line-through",
        color: "#999",
    },
    vazio: {
        textAlign: "center",
        color: "#999",
        marginTop: 24,
        fontSize: 14,
    },
});
