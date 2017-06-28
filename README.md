# Python_Tkinter
# Desenvolvido Por Renan Moreira.
# sistema de agendamento de transporte para viagem. 
import MySQLdb
from Tkinter import*
import Tkinter as tk
import tkMessageBox

conexao = MySQLdb.connect(host='localhost', user='root', passwd='toor',db='dbTransporte')
cursor = conexao.cursor()

LARGE_FONT = ("ARIAL BLACK", 20, 'bold')
def consultar():
	janela= Tk()
	janela.geometry("300x400+300+100")
	janela.mainloop()

		
def sair():
	janela.quit
	
def fechar():
	global app
	app=Raiz()
	app.geometry("700x500+200+100")
	app.title("MRT Sistemas")
	app.mainloop()

def aviso(self):
    self.PaginaRaiz.quit
	

def entra(lo,se):
	log=lo.get()
	sen=se.get()
	if log =="a" and sen =="a":
		janela.withdraw()	
		fechar()
		
		
	else:
		tkMessageBox.showinfo("ERRO","Login/Senha icorreto")

def sobre():
	tkMessageBox.showinfo("MRT SISTEMAS"," Travel Control \n Versao 1.0 \n Desenvolvido por \n Renan Moreira ")
        
class Raiz(tk.Tk):
    def __init__(self, *args, **kwargs):
        tk.Tk.__init__(self, *args, **kwargs)
        container = tk.Frame(self)

        container.pack(side="top", fill="both", expand=True)

        container.grid_rowconfigure(0, weight=1)
        container.grid_columnconfigure(0, weight=1)

        self.frames = {}

        for F in (TelaRaiz, TelaViagem,TelaConsultarViagem, TelaMotorista,TelaConsultarMotorista, TelaVeiculo,TelaConsultarVeiculo, TelaTeste):
            frame = F(container, self)

            self.frames[F] = frame

            frame.grid(row=0, column=0, sticky="nsew")

        self.show_frame(TelaRaiz)

    def show_frame(self, cont):
        frame = self.frames[cont]
        frame.tkraise()


    	###########################
        ##                       ##   
        ##    Tela Principal     ##
	##                       ##
	###########################

class TelaRaiz(tk.Frame):
    
    def __init__(self, parent, controller):
        tk.Frame.__init__(self, parent, bg='#8FBC8F')
        labelTitulo = tk.Label(self, bg='#8FBC8F', text="TRAVEL CONTROL", font=LARGE_FONT)
        labelTitulo.pack(pady=5, padx=10)

        btnViagem = tk.Button(self, bg='#3CB371', width=10, text="Viagem", command=lambda: controller.show_frame(TelaViagem))
        btnViagem.place(x=145, y=100)

        btnMotorista = tk.Button(self, bg='#3CB371', width=10, text="Motorista", command=lambda: controller.show_frame(TelaMotorista))
        btnMotorista.place(x=245, y=100)

        btnVeiculo = tk.Button(self, bg='#3CB371', width=10, text="Veiculo", command=lambda: controller.show_frame(TelaVeiculo))
        btnVeiculo.place(x=345, y=100)
	
	btnSobre = tk.Button(self, bg='#3CB371', width=10, text="Sobre", command=lambda: sobre())
        btnSobre.place(x=445, y=100)

        

	labelTitulo = tk.Label(self, bg='#8FBC8F', text="________________________\n/__|__|___|___|___|___|__|  |\n/           __    //        ****|\n[__(@)_|_|_________(@)__--]", font=LARGE_FONT)
        labelTitulo.pack(pady=150, padx=10)
	

	###########################
        ##                       ##   
        ##      Tela Viagem      ##
	##                       ##
	###########################


class TelaViagem(tk.Frame):
    def __init__(self, parent, controller):
        tk.Frame.__init__(self, parent, bg='#8FBC8F')

        #######    Campos e Botoes    ######


        labelJanelaViagem = tk.Label(self,bg='#8FBC8F', text="AGENDAR VIAGEM", font=('times','25','bold'))
        labelJanelaViagem.pack(pady=10, padx=10)

        lbDataSaida = tk.Label(self, bg='#8FBC8F', text="Data Saida:")
        lbDataSaida.place(x=200, y=200)
        edDataSaida = tk.Entry(self)
        edDataSaida.place(x=300, y=200)

        lbDataChegada = tk.Label(self, bg='#8FBC8F', text="Data Chegada: ")
        lbDataChegada.place(x=200, y=230)
        edDataChegada = tk.Entry(self)
        edDataChegada.place(x=300, y=230)

        lbMotivo = tk.Label(self, bg='#8FBC8F', text="Motivo:")
        lbMotivo.place(x=200, y=260)
        edMotivo = tk.Entry(self)
        edMotivo.place(x=300, y=260)

        lbDestino = tk.Label(self, bg='#8FBC8F', text="Destino:")
        lbDestino.place(x=200, y=290)
        edDestino = tk.Entry(self)
        edDestino.place(x=300, y=290)

        lbMotorista = tk.Label(self, bg='#8FBC8F', text="Motorista:")
        lbMotorista.place(x=200, y=320)
        edMotorista = tk.Entry(self)
        edMotorista.place(x=300, y=320)

        lbVeiculo = tk.Label(self, bg='#8FBC8F', text="Veiculo:")
        lbVeiculo.place(x=200, y=350)
        edVeiculo = tk.Entry(self)
        edVeiculo.place(x=300, y=350)

        ######   posicao dos botoes   ######

	btnSalvar = tk.Button(self,bg='#3CB371', width=10, text="Salvar", command=aviso)
        btnSalvar.place(x=200, y=100)        
	btnAlterar = tk.Button(self, bg='#3CB371', width=10, text="Cosultar", command=lambda: controller.show_frame(TelaConsultarViagem))
        btnAlterar.place(x=300, y=100)
	btnVoltar = tk.Button(self, bg='#3CB371', width=10, text="Voltar", command=lambda: controller.show_frame(TelaRaiz))
        btnVoltar.place(x=400, y=100)
        
	###########################
        ##         Tela          ##   
        ##   Consultar/Alterar   ##
	##	  Viagem      	 ##
	###########################


class TelaConsultarViagem(tk.Frame):
    def __init__(self, parent, controller):
        tk.Frame.__init__(self, parent, bg='#8FBC8F')

        #######    Campos e Botoes    ######


        labelJanelaViagem = tk.Label(self, bg='#8FBC8F', text="ALTERAR / CANCELAR VIAGEM", font=('times','25','bold'))
        labelJanelaViagem.pack(pady=10, padx=10)

        lbDataSaida = tk.Label(self, bg='#8FBC8F', text="Data Saida:")
        lbDataSaida.place(x=200, y=200)
        edDataSaida = tk.Entry(self)
        edDataSaida.place(x=300, y=200)

        lbDataChegada = tk.Label(self, bg='#8FBC8F', text="Data Chegada: ")
        lbDataChegada.place(x=200, y=230)
        edDataChegada = tk.Entry(self)
        edDataChegada.place(x=300, y=230)


        lbMotivo = tk.Label(self, bg='#8FBC8F', text="Motivo:")
        lbMotivo.place(x=200, y=260)
        edMotivo = tk.Entry(self)
        edMotivo.place(x=300, y=260)

        lbDestino = tk.Label(self, bg='#8FBC8F', text="Destino:")
        lbDestino.place(x=200, y=290)
        edDestino = tk.Entry(self)
        edDestino.place(x=300, y=290)

        lbMotorista = tk.Label(self, bg='#8FBC8F', text="Motorista:")
        lbMotorista.place(x=200, y=320)
        edMotorista = tk.Entry(self)
        edMotorista.place(x=300, y=320)

        lbVeiculo = tk.Label(self, bg='#8FBC8F', text="Veiculo:")
        lbVeiculo.place(x=200, y=350)
        edVeiculo = tk.Entry(self)
        edVeiculo.place(x=300, y=350)

        ######   posicao dos botoes   ######

	btnSalvar = tk.Button(self, bg='#3CB371', width=10, text="Alterar", command=aviso)
        btnSalvar.place(x=200, y=100)        
	btnAlterar = tk.Button(self, bg='#3CB371', width=10, text="Cancelar", command=lambda: controller.show_frame(TelaTeste))
        btnAlterar.place(x=300, y=100)
	btnVoltar = tk.Button(self, bg='#3CB371', width=10, text="Voltar", command=lambda: controller.show_frame(TelaViagem))
        btnVoltar.place(x=400, y=100)

	
	###########################
        ##                       ##   
        ##    Tela Motorrista    ##
	##                       ##
	###########################

class TelaMotorista(tk.Frame):
    
    def __init__(self, parent, controller):
        tk.Frame.__init__(self, parent, bg='#8FBC8F')
        labelJanelaMotorista = tk.Label(self, bg='#8FBC8F', text="MOTORISTA", font=('times','25','bold'))
        labelJanelaMotorista.pack(pady=10, padx=10)

        lbNome = tk.Label(self, bg='#8FBC8F', text="Nome:")
        lbNome.place(x=200, y=200)
        nome = tk.Entry(self)
        nome.place(x=300, y=200)

        lbCpf = tk.Label(self, bg='#8FBC8F', text="CPF: ")
        lbCpf.place(x=200, y=230)
        cpf = tk.Entry(self)
        cpf.place(x=300, y=230)

        ######   posicao dos botoes   ######
        
	btnSalvar = tk.Button(self, bg='#3CB371', width=10, text="Salvar", command=lambda:gravar(tk.Entry.get(cpf),tk.Entry.get(nome))) 
        btnSalvar.place(x=200, y=100)
        btnAlterar = tk.Button(self, bg='#3CB371', width=10, text="Cosultar", command=lambda: controller.show_frame(TelaConsultarMotorista))
        btnAlterar.place(x=300, y=100)
	btnVoltar = tk.Button(self, bg='#3CB371', width=10, text="Voltar", command=lambda: controller.show_frame(TelaRaiz))
        btnVoltar.place(x=400, y=100)

	def gravar(cpf,nome):
		insersao1="insert into motorista values(%s,%s)"
		dados1=(cpf,nome)
		cursor.execute(insersao1,dados1)
		conexao.commit()
		apagar()
	def apagar():
 		cpf.delete(0,END)
		nome.delete(0,END)
		tkMessageBox.showinfo("SALVAR","Salvo com sucesso!")
	
		
	

	###########################
        ##         Tela          ##   
        ##   Consultar/Alterar   ##
	##	 Motorista   	 ##
	###########################

class TelaConsultarMotorista(tk.Frame):
    
    def __init__(self, parent, controller):
        tk.Frame.__init__(self, parent, bg='#8FBC8F')
        labelJanelaMotorista = tk.Label(self, bg='#8FBC8F', text="ALTERAR / EXCLUIR MOTORISTA", font=('times','25','bold'))
        labelJanelaMotorista.pack(pady=10, padx=10)

        lbNome = tk.Label(self, bg='#8FBC8F', text="Nome:")
        lbNome.place(x=200, y=200)
        nome = tk.Entry(self)
        nome.place(x=300, y=200)

        lbCpf = tk.Label(self, bg='#8FBC8F', text="CPF: ")

        lbCpf.place(x=200, y=230)
        cpf = tk.Entry(self)
        cpf.place(x=300, y=230)

        ######   posicao dos botoes   ######
        
	btnSalvar = tk.Button(self, bg='#3CB371', width=10, text="Alterar", command=lambda:gravar(tk.Entry.get(cpf),tk.Entry.get(nome))) 
        btnSalvar.place(x=200, y=100)
        btnAlterar = tk.Button(self, bg='#3CB371', width=10, text="Excluir", command=lambda: controller.show_frame(TelaTeste))
        btnAlterar.place(x=300, y=100)
	btnVoltar = tk.Button(self, bg='#3CB371', width=10, text="Voltar", command=lambda: controller.show_frame(TelaMotorista))
        btnVoltar.place(x=400, y=100)

	def gravar(cpf,nome):
		insersao1="insert into motorista values(%s,%s)"
		dados1=(cpf,nome)
		cursor.execute(insersao1,dados1)
		conexao.commit()
		apagar()
	def apagar():
 		cpf.delete(0,END)
		nome.delete(0,END)
		tkMessageBox.showinfo("SALVAR","Salvo com sucesso!")
	def habilitarCampo():
		
		nome1=tk.Entry(self,state='normal')
		nome1.place(x=140, y=200)
		cpf1 = tk.Entry(self,state = 'normal')
       		cpf1.place(x=140, y=230)


	###########################
        ##                       ##   
        ##      Tela Veiculo     ##
	##                       ##
	###########################

        

class TelaVeiculo(tk.Frame):
    def __init__(self, parent, controller):
        tk.Frame.__init__(self, parent, bg='#8FBC8F')
        labelJanelaVeiculo = tk.Label(self, bg='#8FBC8F', text="VEICULO", font=('times','25','bold'))
        labelJanelaVeiculo.pack(pady=10, padx=10)

        lbModelo = tk.Label(self, bg='#8FBC8F', text="Modelo:")
        lbModelo.place(x=200, y=200)
        edModelo = tk.Entry(self)
        edModelo.place(x=300, y=200)

        lbPlaca = tk.Label(self, bg='#8FBC8F', text="Placa: ")
        lbPlaca.place(x=200, y=230)
        edPlaca = tk.Entry(self)
        edPlaca.place(x=300, y=230)

        lbCapacidade = tk.Label(self, bg='#8FBC8F', text="Capacidade:")
        lbCapacidade.place(x=200, y=260)
        edCapacidade = tk.Entry(self)
        edCapacidade.place(x=300, y=260)

        ######   posicao dos botoes   ######

        btnSalvar = tk.Button(self, bg='#3CB371', width=10, text="Salvar", command=lambda:gravar(tk.Entry.get(edModelo),tk.Entry.get(edPlaca),tk.Entry.get(edCapacidade)))
        btnSalvar.place(x=200, y=100)
        btnConsultar = tk.Button(self, bg='#3CB371', width=10, text="Consultar", command=lambda: controller.show_frame(TelaConsultarVeiculo))
        btnConsultar.place(x=300, y=100)
        btnVoltar = tk.Button(self, bg='#3CB371', width=10, text="Voltar", command=lambda: controller.show_frame(TelaRaiz))
        btnVoltar.place(x=400, y=100)

	def gravar(edModelo,edPlaca,edCapacidade):
		insersao2="insert into veiculo values(%s,%s,%s)"
		dados2=(edModelo,edPlaca,edCapacidade)
		cursor.execute(insersao2,dados2)
		conexao.commit()
		apagar()
	def apagar():
 		edModelo.delete(0,END)
		edPlaca.delete(0,END)
		edCapacidade.delete(0,END)
		tkMessageBox.showinfo("SALVAR","Salvo com sucesso!")	




	###########################
        ##         Tela          ##   
        ##   Consultar/Alterar   ##
	##	  Veiculo        ##
	###########################


class TelaConsultarVeiculo(tk.Frame):
    def __init__(self, parent, controller):
        tk.Frame.__init__(self, parent, bg='#8FBC8F')
        labelJanelaVeiculo = tk.Label(self, bg='#8FBC8F', text="ALTERAR / EXCLUIR VEICULO", font=('times','25','bold'))
        labelJanelaVeiculo.pack(pady=10, padx=10)

        lbModelo = tk.Label(self, bg='#8FBC8F', text="Modelo:")
        lbModelo.place(x=200, y=200)
        edModelo = tk.Entry(self)
        edModelo.place(x=300, y=200)

        lbPlaca = tk.Label(self, bg='#8FBC8F', text="Placa: ")
        lbPlaca.place(x=200, y=230)

        edPlaca = tk.Entry(self)
        edPlaca.place(x=300, y=230)

        lbCapacidade = tk.Label(self, bg='#8FBC8F', text="Capacidade:")
        lbCapacidade.place(x=200, y=260)
        edCapacidade = tk.Entry(self)
        edCapacidade.place(x=300, y=260)

        ######   posicao dos botoes   ######

        btnSalvar = tk.Button(self, bg='#3CB371', width=10, text="Alterar", command=lambda: controller.show_frame(PageOne))
        btnSalvar.place(x=200, y=100)
        btnConsultar = tk.Button(self, bg='#3CB371', width=10, text="Excluir", command=lambda: controller.show_frame(TelaTeste))
        btnConsultar.place(x=300, y=100)
        btnVoltar = tk.Button(self, bg='#3CB371', width=10, text="Voltar", command=lambda: controller.show_frame(TelaVeiculo))
        btnVoltar.place(x=400, y=100)




class TelaTeste(tk.Frame):
    def __init__(self, parent, controller):
        tk.Frame.__init__(self, parent, bg='#8FBC8F')
        label = tk.Label(self, bg='#8FBC8F', text="Teste", font=('times','30','bold'))
        label.pack(pady=10, padx=10)

        label = tk.Label(self, bg='#8FBC8F',text="Nesta tela sera implementado o codigo correspondente", font=('times', '15', 'bold'))
        label.pack(pady=10, padx=10)

        btnVoltar = tk.Button(self, bg='#3CB371', width=10, text="Voltar", command=lambda: controller.show_frame(TelaRaiz))
        btnVoltar.place(x=410, y=450)



janela= Tk()
janela.geometry("250x200+500+200")

lt= Label(janela,text="A solucao para seu negocio!")
lt.pack()
lbLogin=Label(janela, text="Login: ")
lbLogin.pack()
lo=Entry(janela,)
lo.pack()
lbSenha =Label(janela, text="Senha: ")
lbSenha.pack()
se=Entry(janela,show="*",)
se.pack()
bt=Button(janela,width=10, bg='#3CB371',text="Entra",command=lambda:entra(lo,se))
bt.pack()
bt=Button(janela,width=10, bg='#3CB371',text="Sair", command=janela.quit)
bt.pack()

janela.title("MRT Sistemas")
janela.mainloop()
