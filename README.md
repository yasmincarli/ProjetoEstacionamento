import java.util.ArrayList;
import java.util.Scanner;

/*
=====================================
 Sistema de Estacionamento - POO em Java
=====================================

 Objetivo:
 ---------
 Simular um estacionamento com funcionários e carros, aplicando
 conceitos de Programação Orientada a Objetos.

 Como executar:
 --------------
 1. Salve como Main.java
 2. Compile: javac Main.java
 3. Rode: java Main

 Conceitos de POO aplicados:
 ---------------------------
 - Classes e Objetos
 - Encapsulamento
 - Construtores parametrizados
 - Herança (Funcionario -> Gerente / Atendente / Caixa)
 - Polimorfismo (sobrescrita mostrarPerfil)
 - Coleções (ArrayList)
 - Entrada/Saída (Scanner)
 - Tratamento de erros (try/catch)
*/

// ===== Classe Funcionario (superclasse) =====
class Funcionario {
    private String nome;
    private String cargo;

    public Funcionario(String nome, String cargo) {
        this.nome = nome;
        this.cargo = cargo;
    }

    public String getNome() { return nome; }
    public String getCargo() { return cargo; }

    // Polimorfismo
    public void mostrarPerfil() {
        System.out.println("Funcionário: " + nome + " | Cargo: " + cargo);
    }
}

// ===== Subclasses =====
class Gerente extends Funcionario {
    public Gerente(String nome) {
        super(nome, "Gerente");
    }

    @Override
    public void mostrarPerfil() {
        System.out.println("👔 Gerente: " + getNome());
    }
}

class Atendente extends Funcionario {
    public Atendente(String nome) {
        super(nome, "Atendente");
    }

    @Override
    public void mostrarPerfil() {
        System.out.println("🙋 Atendente: " + getNome());
    }
}

class Caixa extends Funcionario {
    public Caixa(String nome) {
        super(nome, "Caixa");
    }

    @Override
    public void mostrarPerfil() {
        System.out.println("💰 Caixa: " + getNome());
    }
}

// ===== Classe Carro =====
class Carro {
    private String marca;
    private String modelo;
    private String cor;
    private String placa;

    public Carro(String marca, String modelo, String cor, String placa) {
        this.marca = marca;
        this.modelo = modelo;
        this.cor = cor;
        this.placa = placa;
    }

    public String getPlaca() { return placa; }

    public void mostrarCarro() {
        System.out.println("Carro: " + marca + " " + modelo + " | Cor: " + cor + " | Placa: " + placa);
    }
}

// ===== Classe Estacionamento =====
class Estacionamento {
    private ArrayList<Funcionario> funcionarios;
    private ArrayList<Carro> carros;

    public Estacionamento() {
        funcionarios = new ArrayList<>();
        carros = new ArrayList<>();
    }

    public void adicionarFuncionario(Funcionario f) {
        funcionarios.add(f);
    }

    public void listarFuncionarios() {
        System.out.println("\n--- Funcionários ---");
        for (Funcionario f : funcionarios) {
            f.mostrarPerfil();
        }
    }

    public void entradaCarro(Carro c) {
        carros.add(c);
        System.out.println("🚗 Carro " + c.getPlaca() + " entrou no estacionamento.");
    }

    public void saidaCarro(String placa) {
        Carro removido = null;
        for (Carro c : carros) {
            if (c.getPlaca().equalsIgnoreCase(placa)) {
                removido = c;
                break;
            }
        }
        if (removido != null) {
            carros.remove(removido);
            System.out.println("🚙 Carro " + placa + " saiu do estacionamento.");
        } else {
            System.out.println("❌ Carro não encontrado!");
        }
    }

    public void listarCarros() {
        System.out.println("\n--- Carros no Estacionamento ---");
        for (Carro c : carros) {
            c.mostrarCarro();
        }
    }
}

// ===== Classe Principal =====
public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        Estacionamento est = new Estacionamento();

        int opcao = -1;
        do {
            System.out.println("\n=== MENU ESTACIONAMENTO ===");
            System.out.println("1. Cadastrar Funcionário");
            System.out.println("2. Listar Funcionários");
            System.out.println("3. Registrar Entrada de Carro");
            System.out.println("4. Registrar Saída de Carro");
            System.out.println("5. Listar Carros");
            System.out.println("0. Sair");
            System.out.print("Escolha: ");

            try {
                opcao = sc.nextInt();
                sc.nextLine(); // limpar buffer

                switch(opcao) {
                    case 1:
                        System.out.print("Nome do funcionário: ");
                        String nome = sc.nextLine();
                        System.out.print("Cargo (1-Gerente / 2-Atendente / 3-Caixa): ");
                        int cargo = sc.nextInt(); sc.nextLine();
                        Funcionario f = null;
                        if (cargo == 1) f = new Gerente(nome);
                        else if (cargo == 2) f = new Atendente(nome);
                        else f = new Caixa(nome);

                        est.adicionarFuncionario(f);
                        System.out.println("Funcionário cadastrado!");
                        break;

                    case 2:
                        est.listarFuncionarios();
                        break;

                    case 3:
                        System.out.print("Marca: ");
                        String marca = sc.nextLine();
                        System.out.print("Modelo: ");
                        String modelo = sc.nextLine();
                        System.out.print("Cor: ");
                        String cor = sc.nextLine();
                        System.out.print("Placa: ");
                        String placa = sc.nextLine();

                        Carro c = new Carro(marca, modelo, cor, placa);
                        est.entradaCarro(c);
                        break;

                    case 4:
                        System.out.print("Digite a placa do carro: ");
                        String placaSaida = sc.nextLine();
                        est.saidaCarro(placaSaida);
                        break;

                    case 5:
                        est.listarCarros();
                        break;

                    case 0:
                        System.out.println("Encerrando...");
                        break;
                }

            } catch(Exception e) {
                System.out.println("⚠️ Entrada inválida!");
                sc.nextLine(); // limpar buffer
            }
        } while(opcao != 0);

        sc.close();
    }
}
