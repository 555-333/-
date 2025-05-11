import math

def calculate_pressure_drop():
    # Ввод данных
    density = float(input("Плотность жидкости (кг/м³): "))
    viscosity = float(input("Вязкость жидкости (Па·с): "))
    flow_rate = float(input("Расход жидкости (м³/с): "))
    diameter = float(input("Диаметр трубы (м): "))
    length = float(input("Длина трубы (м): "))
    roughness = float(input("Шероховатость трубы (м): "))

    # Расчет скорости потока
    area = math.pi * (diameter/2)**2
    velocity = flow_rate / area

    # Число Рейнольдса
    re = (density * velocity * diameter) / viscosity

    # Коэффициент трения (формула Дарси-Вейсбаха)
    if re < 2000:
        # Ламинарный поток
        friction_factor = 64 / re
    else:
        # Турбулентный поток (приближение по формуле Коулбрука-Уайта)
        friction_factor = 0.316 / (re ** 0.25)

    # Потери давления
    pressure_drop = (friction_factor * (length/diameter) * 
                    density * velocity**2) / 2

    # Вывод результатов
    print(f"\nРезультаты расчета:")
    print(f"Скорость потока: {velocity:.2f} м/с")
    print(f"Число Рейнольдса: {re:.2f}")
    print(f"Коэффициент трения: {friction_factor:.4f}")
    print(f"Перепад давления: {pressure_drop/1e5:.2f} бар")

if __name__ == "__main__":
    calculate_pressure_drop()
    import matplotlib.pyplot as plt

def plot_flow_profile():
    diameters = [0.1, 0.2, 0.3]
    pressures = [calculate_pressure(d) for d in diameters]
    
    plt.plot(diameters, pressures)
    plt.xlabel('Диаметр трубы (м)')
    plt.ylabel('Давление (Па)')
    plt.title('Зависимость давления от диаметра трубы')
    plt.grid(True)
    plt.show()
def calculate_pump_power(pressure, flow_rate):
    efficiency = 0.85  # КПД насоса
    return (pressure * flow_rate) / efficiency
import sqlite3

def save_to_database(results):
    conn = sqlite3.connect('hydraulics.db')
    c = conn.cursor()
    c.execute('''CREATE TABLE IF NOT EXISTS calculations
                (id INTEGER PRIMARY KEY, pressure REAL, flow REAL)''')
    c.execute("INSERT INTO calculations VALUES (?,?)", 
             (results['pressure'], results['flow']))
    conn.commit()
    conn.close()
