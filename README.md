# Mask-Recognition
Detector de mascara, utlizando o Yolo-v5

É recomendado o uso do google colab com a gpu ativa para o treinamento das imagens, mas sinta-se a 
vontade para fazer por Jupyter, terminal, etc

#Intalação
Peirmeiramente fazemos o clone do github da yolo-v5
e em seguida instalamos os requirementos

```
git clone https://github.com/ultralytics/yolov5
cd yolov5
pip install -r requirements.txt
```

As imagens precisam estar rotuladas no formato da yolo em arquivo txt,
pode-se tanto rotular as imagens por conta própria quanto pegar DataSets prontos.
Para a rotulação recomendo o uso do Roboflow https://roboflow.com/ 
ou o LabelImg https://github.com/heartexlabs/labelImg

Precisamos para o treinamento de um arquivo data.yaml,
nele escrevemos os rotulos do treinamento.
Ex:
```
path: ../data     #Sai da pasta da yolo, e entra no caminho onde estão as imagens e as labels rotuladas

train: imagens_treino
val: imagens_validação
tst: imagens_teste      #Sinta-se a vontade em colocar imagens de testa ou não

nc: 2   #Quatidade de Classes

names: ['No mask' , 'Mask']     #Colocar na ordem em em que foi rotulado, se 'No mask' tiver o (0) como primeiro termo no arquivo txt,
precisará também ser o primeiro termo em relação a quantidade de classes
```

### Resta agora realizar o treinamento.

```
!python train.py --img 640 --batch 20 --epochs 100 --data data.yaml --weights yolov5s.pt --cache
```
São gerados dois arquivos, o best.pt (recomendado) que gera o melhor resultado obtido no treinamento e o last.pt, que é o último resultado gerado.
O resultado do treinamento fica disponível em yolov5/runs/train/exp/weights/best.pt, é necessário mover o arquivo gerado para a pasta yolov5, se preferir pode renomeá-lo
Para fazer um novo treinamento a partir do último resultado, basta mudar o nome do treinamento passado 'yolov5s.pt' para o novo resultado 'best.pt'

Caso esteja usando o notebook do Google Colab e seu DataSet possua muitas imagens, recomendo que faça uma divisão de até 500 imagens por treinamento.

### Para testar o modelo, pode ser com a Webcan do computador, youtube, vídeos salvos e imagens
```
python3 detect.py --conf 0.5 --weights model.pt --source 0    #Irá exibir resultados a partir de 50% de precisão e o source da camera é referente a porta de entrada do computador
```

