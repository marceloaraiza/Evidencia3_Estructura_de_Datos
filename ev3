import datetime
import sqlite3
import sys
import openpyxl
from sqlite3 import Error
from tabulate import tabulate

list_salas=[]
list_clientes=[]
list_reservaciones=[]
lista_reporte=[]
lista_reporte_excel=[]


def capturafechareservacion():
    while True:
        cadena_fecha_reservacion = input("INGRESE LA FECHA DE RESERVACIÓN EN EL FORMATO (DD/MM/AAAA): \n")
        try:
            fecha_reservacion = datetime.datetime.strptime(cadena_fecha_reservacion, "%d/%m/%Y")
            fecha_reservacion_procesada = (fecha_reservacion - datetime.timedelta(days=+2)).date()
            fecha_actual = datetime.date.today()
            if fecha_reservacion_procesada>=fecha_actual:
                break
            else:
                print("LA RESERVACIONES SOLO SE PUEDEN REALIZAR COMO MINIMO CON DOS DIAS DE ANTICIPACIÓN. ")
                continue

        except Exception:
            print(f"SE PRODUJO EL SIGUIENTE ERROR: {sys.exc_info()[0]}")
    return fecha_reservacion


def listado_reservaciones():
    try:
        with sqlite3.connect("reservaciones.db",detect_types = sqlite3.PARSE_DECLTYPES | sqlite3.PARSE_COLNAMES) as conn:
            mi_cursor = conn.cursor()
            mi_cursor.execute("SELECT * FROM reservacion")
            registros = mi_cursor.fetchall()
            if registros:
                for clave, nombre_evento,turno,fecha_reservacion,num_cliente,num_sala,nombre_sala in registros:
                    reservacion=(clave, nombre_evento,turno,fecha_reservacion.date().strftime('%d/%m/%Y'),num_cliente,num_sala,nombre_sala)
                    list_reservaciones.append(reservacion)
                print (tabulate(list(list_reservaciones),headers=["NUM RESERVACION","NOMBRE EVENTO","TURNO","FECHA RESERVACION","NUM CLIENTE","NUM SALA","NOMBRE SALA"],tablefmt='grid'))
                print("")
                list_reservaciones.clear()
            else:
                print("NO SE ENCONTRARON REGISTROS.")
    except Error as e:
        print (e)
    except Exception:
        print(f"SE PRODUJO EL SIGUIENTE ERROR: {sys.exc_info()[0]}")
    finally:
        conn.close()

def listado_salas():
    try:
        with sqlite3.connect("reservaciones.db") as conn:
            mi_cursor = conn.cursor()
            mi_cursor.execute("SELECT * FROM sala")
            registros = mi_cursor.fetchall()
            if registros:
                for clave,nombre,capacidad in registros:
                    sala=(clave,nombre,capacidad)
                    list_salas.append(sala)
                print (tabulate(list(list_salas),headers=["NUM SALA","NOMBRE SALA","CAPACIDAD"],tablefmt='grid'))
                print("")
                list_salas.clear()
            else:
                print("NO SE ENCONTRARON REGISTROS.")
    except Error as e:
        print (e)
    except Exception:
        print(f"SE PRODUJO EL SIGUIENTE ERROR: {sys.exc_info()[0]}")
    finally:
        conn.close()

def listado_clientes():
    try:
        with sqlite3.connect("reservaciones.db") as conn:
            mi_cursor = conn.cursor()
            mi_cursor.execute("SELECT * FROM cliente")
            registros = mi_cursor.fetchall()
            if registros:
                for clave, nombre in registros:
                    cliente=(clave,nombre)
                    list_clientes.append(cliente)
                print (tabulate(list(list_clientes),headers=["NUM CLIENTE","NOMBRE"],tablefmt='grid'))
                print("")
                list_clientes.clear()
            else:
                print("NO SE ENCONTRARON REGISTROS EN LA RESPUESTA.")
    except Error as e:
        print (e)
    except Exception:
        print(f"SE PRODUJO EL SIGUIENTE ERROR: {sys.exc_info()[0]}")
    finally:
        conn.close()

def menu_principal():
    print("xxxxxxxxxxxxMENU PRINCIPALxxxxxxxxxxxx")
    print("[1] RESERVACIONES.")
    print("[2] REPORTES.")
    print("[3] REGISTRAR UNA NUEVA SALA.")
    print("[4] REGISTRAR UN NUEVO CLIENTE.")
    print("[5] SALIR.")



try:
    with sqlite3.connect("reservaciones.db") as conexion:
        mi_cursor = conexion.cursor()
        mi_cursor.execute("CREATE TABLE IF NOT EXISTS cliente (num_cliente INTEGER PRIMARY KEY, nombre TEXT NOT NULL);")
        mi_cursor.execute("CREATE TABLE IF NOT EXISTS sala (num_sala INTEGER PRIMARY KEY, nombre TEXT NOT NULL,capacidad INTEGER);")
        mi_cursor.execute("CREATE TABLE IF NOT EXISTS reservacion (num_reservacion INTEGER PRIMARY KEY, nombre TEXT NOT NULL,turno TEXT NOT NULL,fecha_reservacion timestamp NOT NULL,num_cliente INTEGER NOT NULL,num_sala INTEGER NOT NULL,nombre_sala TEXT NOT NULL,FOREIGN KEY (num_cliente) REFERENCES cliente (num_cliente),FOREIGN KEY (num_sala) REFERENCES sala (num_sala));")
except sqlite3.Error as e:
    print(e)
except:
    print(f"SE PRODUJO EL SIGUIENTE ERROR: {sys.exc_info()[0]}")
finally:
    if (conexion):
        conexion.close()


while True:
    menu_principal()
    while True:
        dato_menu = input("INGRESE EL NUMERO DE OPCION DEL MENU PRINCIPAL QUE SE DESEA REALIZAR, INGRESE SOLAMENTE NUMEROS: \n")
        try:
            opcion_principal = int(dato_menu)
            break
        except Exception:
            print(f"SE PRODUJO EL SIGUIENTE ERROR: {sys.exc_info()[0]}")

    if opcion_principal==1:
        while True:
            print("")
            print("*****SUBMENU RESERVACIONES*****")
            print("[1] REGISTRAR UNA RESERVACION.")
            print("[2] MODIFICAR NOMBRE DE UNA RESERVACION.")
            print("[3] CONSULTAR DISPONIBILIDAD DE SALAS PARA UNA FECHA.")
            print("[4] ELIMINAR UNA RESERVACION.")
            print("[5] REGRESAR AL MENU PRINCIPAL.")
            while True:
                dato_submenu = input("INGRESE EL NUMERO DE OPCION DEL SUBMENU QUE SE DESEA REALIZAR, INGRESE SOLAMENTE NUMEROS: \n")
                try:
                    opcion_submenu = int(dato_submenu)
                    break
                except Exception:
                    print(f"SE PRODUJO EL SIGUIENTE ERROR: {sys.exc_info()[0]}")
            if opcion_submenu==1:
                while True:
                    try:
                        with sqlite3.connect("reservaciones.db") as conn:
                            mi_cursor = conn.cursor()
                            mi_cursor.execute("SELECT max(num_reservacion) FROM reservacion")
                            registros = mi_cursor.fetchall()
                            if registros:
                                clave_max=registros[0][0]
                                if clave_max==None:
                                    clave=1
                                else:
                                    clave=clave_max+1
                    except Error as e:
                        print (e)
                    except Exception:
                        print(f"SE PRODUJO EL SIGUIENTE ERROR: {sys.exc_info()[0]}")
                    finally:
                        conn.close()
                    break

                while True:
                    print("SALAS REGISTRADAS: ")
                    listado_salas()
                    sala=input("INGRESE EL NUMERO DE LA SALA EN DONDE SE HARA LA RESERVACIÓN: \n")
                    try:
                        if sala=="":
                            print("EL NUMERO DE SALA NO DEBE OMITIRSE.")
                            continue
                        num_sala = int(sala)
                    except Exception:
                        print(f"SE PRODUJO EL SIGUIENTE ERROR: {sys.exc_info()[0]}")
                        print("SOLO SE PERMITEN NUMEROS ENTEROS.")

                    try:
                        with sqlite3.connect("reservaciones.db") as conn:
                            mi_cursor = conn.cursor()
                            valores = {"clave":num_sala}
                            mi_cursor.execute("SELECT num_sala,nombre FROM sala where num_sala=:clave",valores)
                            registro = mi_cursor.fetchall()
                            if registro:
                                num_sala=registro[0][0]
                                nombre_sala=registro[0][1]
                                break
                            else:
                                print("EL NUMERO DE SALA NO EXISTE.")
                    except Error as e:
                        print (e)
                    except Exception:
                        print(f"SE PRODUJO EL SIGUIENTE ERROR: {sys.exc_info()[0]}")
                    finally:
                        conn.close()

                while True:
                    print("CLIENTES REGISTRADOS: ")
                    listado_clientes()
                    cliente=input("INGRESE EL NUMERO DEL CLIENTE QUE REALIZA LA RESERVACIÓN: \n")
                    try:
                        if cliente=="":
                            print("EL NUMERO DEL CLIENTE NO DEBE OMITIRSE.")
                            continue
                        num_cliente = int(cliente)
                    except Exception:
                        print(f"LO SIENTO :( OCURRIO UNA EXCEPCIÓN DE TIPO: {sys.exc_info()[0]}")
                        print("SOLO SE PERMITEN NUMEROS ENTEROS.")

                    try:
                        with sqlite3.connect("reservaciones.db") as conn:
                            mi_cursor = conn.cursor()
                            valores = {"clave":num_cliente}
                            mi_cursor.execute("SELECT num_cliente FROM cliente where num_cliente=:clave",valores)
                            registro = mi_cursor.fetchall()
                            if registro:
                                num_cliente=registro[0][0]
                                break
                            else:
                                print("EL NUMERO DE SALA NO EXISTE.")
                    except Error as e:
                        print (e)
                    except Exception:
                        print(f"SE PRODUJO EL SIGUIENTE ERROR: {sys.exc_info()[0]}")
                    finally:
                        conn.close()

                while True:
                    turno = input("INGRESE EL TURNO DEL EVENTO: [MATUTINO] [VESPERTINO] [NOCTURNO] \n").upper()
                    if (turno ==""):
                        print("EL TURNO NO DEBE OMITIRSE")
                        continue
                    elif (turno =="MATUTINO") or (turno =="VESPERTINO") or (turno =="NOCTURNO"):
                        print("")
                        break
                    else:
                        print("TURNO NO DISPONIBLE.")

                fecha_reservacion=capturafechareservacion()

                while True:
                    nombre_evento=input("INGRESE EL NOMBRE DEL EVENTO: \n").upper()
                    if (nombre_evento ==""):
                        print("EL NOMBRE DEL EVENTO NO DEBE OMITIRSE")
                        continue
                    else:
                        break

                try:
                    with sqlite3.connect("reservaciones.db") as conn:
                        mi_cursor = conn.cursor()
                        criterios={"turno":turno,"fecha_reservacion":fecha_reservacion,"num_sala":num_sala}
                        mi_cursor.execute("SELECT * FROM reservacion where turno=:turno and fecha_reservacion=:fecha_reservacion and num_sala=:num_sala",criterios)
                        registro = mi_cursor.fetchall()
                        if registro:
                            print("NO SE PUEDEN REALIZAR DOS RESERVACIONES SIMULTANEAMENTE.")
                            break
                except Error as e:
                    print (e)
                except Exception:
                    print(f"Se produjo el siguiente error: {sys.exc_info()[0]}")
                finally:
                    conn.close()

                try:
                    with sqlite3.connect("reservaciones.db") as conn:
                        mi_cursor = conn.cursor()
                        mi_cursor.execute("PRAGMA FOREIGN_KEYS=ON;")
                        valores_reservacion=(clave,nombre_evento,turno,fecha_reservacion,num_cliente,num_sala,nombre_sala)
                        mi_cursor.execute("INSERT INTO reservacion VALUES(?,?,?,?,?,?,?)", valores_reservacion)
                        print("REGISTRO AGREGADO CORRECTAMENTE.")
                        print("")
                except Error as e:
                    print (e)
                except:
                    print(f"SE PRODUJO EL SIGUIENTE ERROR: {sys.exc_info()[0]}")
                finally:
                    conn.close()
                break
            elif opcion_submenu==2:
                while True:
                    print("LISTADO DE RESERVACIONES: ")
                    listado_reservaciones()
                    while True:
                        str_llave_reservaciones = input("INGRESE EL NUMERO DE RESERVACION QUE SE VA MODIFICAR : \n")
                        if str_llave_reservaciones=="":
                            break
                        else:
                            try:
                                llave_reservaciones = int(str_llave_reservaciones)
                                break
                            except Exception:
                                print(f"SE PRODUJO EL SIGUIENTE ERROR: {sys.exc_info()[0]}")

                    try:
                        with sqlite3.connect("reservaciones.db") as conn:
                            mi_cursor = conn.cursor()
                            valores = {"num_reservacion":llave_reservaciones}
                            mi_cursor.execute("SELECT num_reservacion FROM reservacion where num_reservacion=:num_reservacion",valores)
                            registro = mi_cursor.fetchall()
                            if registro:
                                clave_reservacion=registro[0][0]
                            else:
                                print("EL NUMERO DE RESERVACION NO EXISTE.")
                                break
                    except Error as e:
                        print (e)
                    except Exception:
                        print(f"SE PRODUJO EL SIGUIENTE ERROR: {sys.exc_info()[0]}")
                    finally:
                        conn.close()

                    while True:
                        nuevo_nombre=input("INGRESE EL NUEVO NOMBRE DEL EVENTO: \n").upper()
                        if (nuevo_nombre ==""):
                            print("EL NOMBRE DEL EVENTO NO DEBE OMITIRSE")
                            continue
                        else:
                            break
                    try:
                        with sqlite3.connect("reservaciones.db") as conn:
                            mi_cursor = conn.cursor()
                            valores = (nuevo_nombre, clave_reservacion)
                            mi_cursor.execute("UPDATE reservacion SET nombre=(?) WHERE num_reservacion=(?);",valores)
                            print("REGISTRO ACTUALIZADO CORRECTAMENTE.")
                    except Error as e:
                        print (e)
                    except:
                        print(f"SE PRODUJO EL SIGUIENTE ERROR: {sys.exc_info()[0]}")
                    finally:
                        if (conn):
                            conn.close()
                            break
            elif opcion_submenu==3:
                while True:
                    cadena_fecha_disp = input("INGRESE LA FECHA PARA CONSULTAR DISPONIBILIDAD DE SALAS EN EL FORMATO (DD/MM/AAAA): \n")
                    try:
                        fecha_valida = datetime.datetime.strptime(cadena_fecha_disp, "%d/%m/%Y")
                        fecha_disp = fecha_valida.date()
                        break
                    except Exception:
                        print(f"SE PRODUJO EL SIGUIENTE ERROR: {sys.exc_info()[0]}")
                        print("")
                        continue

                salas_ocupadas=set()
                posibles_salas_disp=set()
                try:
                    with sqlite3.connect("reservaciones.db", detect_types = sqlite3.PARSE_DECLTYPES | sqlite3.PARSE_COLNAMES) as conn:
                        mi_cursor = conn.cursor()
                        criterios = (fecha_disp,)
                        mi_cursor.execute("SELECT num_sala,nombre_sala,turno FROM reservacion WHERE DATE(fecha_reservacion)=(?);", criterios)
                        registros = mi_cursor.fetchall()

                        for num_sala,nombre_sala,turno in registros:
                            salas_reservada=(num_sala,nombre_sala,turno)
                            salas_ocupadas.add(salas_reservada)

                except sqlite3.Error as e:
                    print (e)
                except Exception:
                    print(f"SE PRODUJO EL SIGUIENTE ERROR: {sys.exc_info()[0]}")
                finally:
                    if (conn):
                        conn.close()
                turnos = ["MATUTINO","VESPERTINO","NOCTURNO"]

                try:
                    with sqlite3.connect("reservaciones.db") as conn:
                        mi_cursor = conn.cursor()
                        mi_cursor.execute("SELECT * FROM sala ;")
                        registros = mi_cursor.fetchall()

                        for num_sala,nombre_sala,capacidad in registros:
                            for turno in turnos:
                                salas_disp=(num_sala,nombre_sala,turno)
                                posibles_salas_disp.add(salas_disp)
                except sqlite3.Error as e:
                    print (e)
                except Exception:
                    print(f"SE PRODUJO EL SIGUIENTE ERROR: {sys.exc_info()[0]}")
                finally:
                    if (conn):
                        conn.close()
                salas_disponibles=posibles_salas_disp-salas_ocupadas
                list_salas_disp = list(sorted(salas_disponibles))
                print("")
                print(f"REPORTE DE SALAS DISPONIBLES PARA RESERVAR DE LA FECHA: {fecha_disp.strftime('%d/%m/%Y')}.")
                print (tabulate(list(list_salas_disp),headers=["NUMERO SALA","NOMBRE SALA","TURNO"],tablefmt='grid'))

            elif opcion_submenu==4:
                while True:
                    print("LISTADO DE RESERVACIONES: ")
                    listado_reservaciones()
                    while True:
                        str_llave_reservaciones = input("INGRESE EL NUMERO DE RESERVACION QUE SE QUIERE ELIMINAR: \n")
                        if str_llave_reservaciones=="":
                            break
                        else:
                            try:
                                llave_reservaciones = int(str_llave_reservaciones)
                                break
                            except Exception:
                                print(f"SE PRODUJO EL SIGUIENTE ERROR: {sys.exc_info()[0]}")

                    try:
                        with sqlite3.connect("reservaciones.db") as conn:
                            mi_cursor = conn.cursor()
                            valores = {"num_reservacion":llave_reservaciones}
                            mi_cursor.execute("SELECT num_reservacion FROM reservacion where num_reservacion=:num_reservacion",valores)
                            registro = mi_cursor.fetchall()
                            if registro:
                                clave_reservacion=registro[0][0]
                            else:
                                print("EL NUMERO DE RESERVACION NO EXISTE.")
                                break
                    except Error as e:
                        print (e)
                    except Exception:
                        print(f"SE PRODUJO EL SIGUIENTE ERROR: {sys.exc_info()[0]}")
                    finally:
                        conn.close()

                    try:
                        with sqlite3.connect("reservaciones.db",detect_types = sqlite3.PARSE_DECLTYPES | sqlite3.PARSE_COLNAMES) as conn:
                            mi_cursor = conn.cursor()
                            valores = {"num_reservacion":llave_reservaciones}
                            mi_cursor.execute("SELECT * FROM reservacion where num_reservacion=:num_reservacion",valores)
                            registro = mi_cursor.fetchall()
                            if registro:
                                for clave, nombre_evento,turno,fecha_reservacion,num_cliente,num_sala,nombre_sala in registro:
                                    reservacion=(clave, nombre_evento,turno,fecha_reservacion.date().strftime('%d/%m/%Y'),num_cliente,num_sala,nombre_sala)
                                list_reservaciones.append(reservacion)
                                print (tabulate(list(list_reservaciones),headers=["NUM RESERVACION","NOMBRE EVENTO","TURNO","FECHA RESERVACION","NUM CLIENTE","NUM SALA","NOMBRE SALA"],tablefmt='grid'))
                                print("")
                                list_reservaciones.clear()
                    except Error as e:
                        print (e)
                    except Exception:
                        print(f"SE PRODUJO EL SIGUIENTE ERROR: {sys.exc_info()[0]}")
                    finally:
                        conn.close()

                    while True:
                        print("NOTA: UNA VES ELIMINADA LA RESERVACION LOS DATOS NO PODRAN RECUPERARSE.")
                        confirmacion=input("¿DESEA ELIMINAR LA RESERVACION QUE CONTIENE LOS SIGUIENTES DATOS? INGRESE [1] PARA CONFIRMAR, DEJE VACIO PARA SALIR: ")
                        if confirmacion=="":
                            break
                        try:
                            confirmacion = int(confirmacion)
                            break
                        except Exception:
                            print(f"SE PRODUJO EL SIGUIENTE ERROR: {sys.exc_info()[0]}")

                    if confirmacion==1:
                        try:
                            with sqlite3.connect("reservaciones.db",detect_types = sqlite3.PARSE_DECLTYPES | sqlite3.PARSE_COLNAMES) as conn:
                                mi_cursor = conn.cursor()
                                valores = {"num_reservacion":llave_reservaciones}
                                mi_cursor.execute("SELECT fecha_reservacion FROM reservacion where num_reservacion=:num_reservacion",valores)
                                registro = mi_cursor.fetchall()
                                if registro:
                                    fecha_reservacion=registro[0][0]
                                else:
                                    print("EL NUMERO DE RESERVACION NO EXISTE.")
                        except Error as e:
                            print (e)
                        except Exception:
                            print(f"SE PRODUJO EL SIGUIENTE ERROR: {sys.exc_info()[0]}")
                        finally:
                            conn.close()

                        try:
                            fecha_reservacion_procesada = (fecha_reservacion - datetime.timedelta(days=+3)).date()
                            fecha_actual = datetime.date.today()
                            if fecha_reservacion_procesada>=fecha_actual:
                                try:
                                    with sqlite3.connect("reservaciones.db") as conn:
                                        mi_cursor = conn.cursor()
                                        valores = (clave_reservacion,)
                                        mi_cursor.execute("DELETE FROM reservacion WHERE num_reservacion=(?);",valores)
                                        print("REGISTRO ELIMINADO CORRECTAMENTE.")
                                        break
                                except Error as e:
                                    print (e)
                                except:
                                    print(f"SE PRODUJO EL SIGUIENTE ERROR: {sys.exc_info()[0]}")
                                finally:
                                    if (conn):
                                        conn.close()
                            else:
                                print("LA RESERVACIONES SOLO SE PUEDEN ELIMINAR COMO MINIMO CON 3 DIAS DE ANTICIPACIÓN. ")
                                break
                        except Exception:
                            print(f"SE PRODUJO EL SIGUIENTE ERROR: {sys.exc_info()[0]}")
                    else:
                        break
            elif opcion_submenu==5:
                break
    elif opcion_principal==2:
        while True:
            print("")
            print("*****SUBMENU REPORTES*****")
            print("[1] REPORTE EN PANTALLA DE RESERVACIONES.")
            print("[2] EXPORTAR REPORTE TABULAR EN EXCEL.")
            print("[3] SALIR.")
            while True:
                dato_submenu = input("INGRESE EL NUMERO DE OPCION DEL SUBMENU QUE SE DESEA REALIZAR, INGRESE SOLAMENTE NUMEROS: \n")
                try:
                    opcion_submenu2 = int(dato_submenu)
                    break
                except Exception:
                    print(f"SE PRODUJO EL SIGUIENTE ERROR: {sys.exc_info()[0]}")
            if opcion_submenu2==1:
                while True:
                    cadena_fecha_disp = input("INGRESE LA FECHA EN EL FORMATO (DD/MM/AAAA) PARA GENERAR UN REPORTE DE RESERVACIONES EN PANTALLA: \n")
                    try:
                        fecha_valida = datetime.datetime.strptime(cadena_fecha_disp, "%d/%m/%Y")
                        fecha_consulta = fecha_valida.date()
                        break
                    except Exception:
                        print(f"SE PRODUJO EL SIGUIENTE ERROR: {sys.exc_info()[0]}")
                        print("")
                        continue

                try:
                    with sqlite3.connect("reservaciones.db", detect_types = sqlite3.PARSE_DECLTYPES | sqlite3.PARSE_COLNAMES) as conn:
                        mi_cursor = conn.cursor()
                        criterios = (fecha_consulta,)
                        mi_cursor.execute("SELECT * FROM reservacion WHERE DATE(fecha_reservacion)=(?);", criterios)
                        registros = mi_cursor.fetchall()

                        if registros:
                            for clave, nombre_evento,turno,fecha_reservacion,num_cliente,num_sala,nombre_sala in registros:
                                reservacion=(clave, nombre_evento,turno,fecha_reservacion.date().strftime('%d/%m/%Y'),num_cliente,num_sala,nombre_sala)
                                lista_reporte.append(reservacion)

                            print (tabulate(list(lista_reporte),headers=["NUM RESERVACION","NOMBRE EVENTO","TURNO","FECHA RESERVACION","NUM CLIENTE","NUM SALA","NOMBRE SALA"],tablefmt='grid'))
                            print("")
                            lista_reporte.clear()

                        else:
                            print(f"AUN NO HAY RESERVACIONES EN LA FECHA: {fecha_consulta.strftime('%d/%m/%Y')} ")

                except sqlite3.Error as e:
                    print (e)
                except Exception:
                    print(f"SE PRODUJO EL SIGUIENTE ERROR: {sys.exc_info()[0]}")
                finally:
                    if (conn):
                        conn.close()

            elif opcion_submenu2==2:
                while True:
                    cadena_fecha_disp = input("INGRESE LA FECHA EN EL FORMATO (DD/MM/AAAA) PARA EXPORTAR UN REPORTE DE RESERVACIONES EN EXCEL: \n")
                    try:
                        fecha_valida = datetime.datetime.strptime(cadena_fecha_disp, "%d/%m/%Y")
                        fecha_consulta = fecha_valida.date()
                        break
                    except Exception:
                        print(f"SE PRODUJO EL SIGUIENTE ERROR: {sys.exc_info()[0]}")
                        print("")
                        continue

                lista_reporte_excel.clear()
                try:
                    with sqlite3.connect("reservaciones.db", detect_types = sqlite3.PARSE_DECLTYPES | sqlite3.PARSE_COLNAMES) as conn:
                        mi_cursor = conn.cursor()
                        criterios = (fecha_consulta,)
                        mi_cursor.execute("SELECT * FROM reservacion WHERE DATE(fecha_reservacion)=(?);", criterios)
                        registros = mi_cursor.fetchall()

                        if registros:
                            for clave, nombre_evento,turno,fecha_reservacion,num_cliente,num_sala,nombre_sala in registros:
                                reservacion=(clave, nombre_evento,turno,fecha_reservacion.date().strftime('%d/%m/%Y'),num_cliente,num_sala,nombre_sala)
                                lista_reporte_excel.append(reservacion)

                            libro = openpyxl.Workbook()
                            hoja = libro["Sheet"]
                            hoja.title = "REPORTE"
                            hoja.append(('NUM RESERVACION','NOMBRE EVENTO','TURNO','FECHA','NUM CLIENTE','NUM SALA','NOMBRE SALA'))
                            for reporte in lista_reporte_excel:
                                hoja.append(reporte)

                            libro.save("ReporteReservaciones.xlsx")
                            print("SE EXPORTO EL REPORTE TABULAR A UN ARCHIVO DE EXCEL.")

                        else:
                            print(f"AUN NO HAY RESERVACIONES EN LA FECHA: {fecha_consulta.strftime('%d/%m/%Y')} ")

                except sqlite3.Error as e:
                    print (e)
                except Exception:
                    print(f"SE PRODUJO EL SIGUIENTE ERROR: {sys.exc_info()[0]}")
                finally:
                    if (conn):
                        conn.close()


            elif opcion_submenu2==3:
                break
            else:
                print("OPCION NO DISPONIBLE.")

    elif opcion_principal==3:
        while True:
            try:
                with sqlite3.connect("reservaciones.db") as conn:
                    mi_cursor = conn.cursor()
                    mi_cursor.execute("SELECT max(num_sala) FROM sala")
                    registros = mi_cursor.fetchall()
                    if registros:
                        clave_max=registros[0][0]
                        if clave_max==None:
                            clave=1
                        else:
                            clave=clave_max+1
            except Error as e:
                print (e)
            except Exception:
                print(f"SE PRODUJO EL SIGUIENTE ERROR: {sys.exc_info()[0]}")
            finally:
                conn.close()
            while True:
                nombre_sala = input("INGRESE EL NOMBRE DE LA SALA QUE SE VA REGISTRAR: \n").upper()
                if (nombre_sala==""):
                    print("EL NOMBRE DE LA SALA NO DEBE OMITIRSE.")
                    continue
                else:
                    break

            while True:
                cap_sala = input("INGRESE LA CAPACIDAD DE PERSONAS DE LA SALA: \n")
                try:
                    if cap_sala == "":
                        print("EL CUPO DE LA SALA NO DEBE OMITIRSE.")
                    cupo_sala = int(cap_sala)
                    if cupo_sala<1:
                        print("EL DATO DEBE SER MAYOR QUE CERO.")
                        continue
                    break
                except Exception:
                    print(f"LO SIENTO :( OCURRIO UNA EXCEPCIÓN DE TIPO: {sys.exc_info()[0]}")
            try:
                with sqlite3.connect("reservaciones.db") as conn:
                    mi_cursor = conn.cursor()
                    valores = (clave,nombre_sala,cap_sala)
                    mi_cursor.execute("INSERT INTO sala VALUES(?,?,?)", valores)
                    print("REGISTRO AGREGADO CORRECTAMENTE.")
                    print("")
            except Error as e:
                print (e)
            except:
                print(f"SE PRODUJO EL SIGUIENTE ERROR: {sys.exc_info()[0]}")
            finally:
                conn.close()
            break
    elif opcion_principal==4:
        try:
            with sqlite3.connect("reservaciones.db") as conn:
                mi_cursor = conn.cursor()
                mi_cursor.execute("SELECT max(num_cliente) FROM cliente")
                registros = mi_cursor.fetchall()

                if registros:
                    clave_max=registros[0][0]
                    if clave_max==None:
                        clave=1
                    else:
                        clave=clave_max+1
        except Error as e:
            print (e)
        except Exception:
            print(f"SE PRODUJO EL SIGUIENTE ERROR: {sys.exc_info()[0]}")
        finally:
            conn.close()

        while True:
            nombre_cliente = input("INGRESE EL NOMBRE DEL CLIENTE QUE SE VA REGISTRAR: \n").upper()
            if (nombre_cliente==""):
                print("EL NOMBRE DEL CLIENTE NO DEBE OMITIRSE.")
                continue
            else:
                try:
                    with sqlite3.connect("reservaciones.db") as conn:
                        mi_cursor = conn.cursor()
                        valores = (clave, nombre_cliente)
                        mi_cursor.execute("INSERT INTO cliente VALUES(?,?)", valores)
                        print("REGISTRO AGREGADO CORRECTAMAENTE.")
                        print("")
                except Error as e:
                    print (e)
                except:
                    print(f"SE PRODUJO EL SIGUIENTE ERROR: {sys.exc_info()[0]}")
                finally:
                    conn.close()
            break
    elif opcion_principal==5:
        break
    else:
        print("OPCION DEL MENU NO DISPONIBLE.")
