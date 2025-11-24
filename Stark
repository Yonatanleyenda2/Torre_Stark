import dash
from dash import html, dcc, Input, Output
import pandas as pd
import plotly.express as px
import dash_bootstrap_components as dbc

# ============================
# DATOS DE EJEMPLO
# ============================
df = pd.DataFrame({
    "ZONA": ["P1_CASA", "P1_LOCAL", "P2_H1", "P2_H2", "P2_H3", "P2_H4","P2_H5","P2_H6","P2_H7","P2_H8","P2_H9","P3_H1","P3_H2","P3_H3","P3_H4","P3_H5","P3_H6","P3_H7","P3_H8","P3_H9",],
    "Precio": [5000,550,550,550,450,550,450,550,450,550,550,600,600,600,600,600,600,600,600,600,],
    "PISO": [1,1,2,2,2,2,2,2,2,2,2,3,3,3,3,3,3,3,3,3,],
    "OCUPACION": ["SI","SI","SI","SI","NO","SI","SI","SI","SI","NO","SI","NO","NO","NO","NO","NO","NO","NO","NO","NO",],
   })
                
# ============================
# DASH APP
# ============================
app = dash.Dash(__name__)

app.layout = html.Div(
    style={"padding": "20px", "font-family": "Arial"},
    children=[
        html.H1("Dashboard de Arriendos Torre STARK"),

        html.Label("Filtrar por estrato:"),
        dcc.Dropdown(
            id="estrato_dd",
            options=[{"label": str(e), "value": e} for e in sorted(df["PISO"].unique())],
            value=None,
            placeholder="Seleccione un estrato"
        ),

        dcc.Graph(id="precio_graf"),

        html.H3("Tabla de resultados"),
        html.Div(id="tabla_div"),

        html.H3("Valor Total MES"),
        html.Div(id="valor_total", style={"font-size": "24px", "font-weight": "bold"})
    ]
)


@app.callback(
    [Output("precio_graf", "figure"),
     Output("tabla_div", "children"),
     Output("valor_total", "children")],
    Input("estrato_dd", "value")
)
def actualizar_tablero(estrato):
    suma=[]
    if estrato:
        dff = df[df["PISO"] == estrato]
        
    else:
        dff = df

    fig = px.bar(
        dff,
        x="LOCAL-HABITACIONES",
        y="Precio",
        title="Precio promedio por barrio",
        labels={"Precio": "Precio (COP)"}
    )

    tabla = html.Table([
        html.Tr([html.Th(col) for col in dff.columns])] +
        [html.Tr([html.Td(dff.iloc[i][col]) for col in dff.columns])
         for i in range(len(dff))]
    )
    if dff.empty==False:
        for w in range(len(dff)):
            if dff.iloc[w]["OCUPACION"]=="SI":
               suma.append(float(dff.iloc[w]["Precio"]))         
        Valor=sum(suma)*1000
        print("Valor ",Valor)
    else:
        Valor=0
    

    return fig, tabla, f"${Valor:,.0f}"


if __name__ == "__main__":
    app.run_server(debug=True, host="127.0.0.1", port=8050)
