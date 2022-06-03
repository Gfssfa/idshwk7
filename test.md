# isdhwk7

## 1. folder structure

| file name  | description                               |
| ---------- | ----------------------------------------- |
| origin.jpg | original picture                          |
| test.png | picture modified with hidden information  |
| info.txt   | hide information                          |
| info2.txt  | information decoded from modified picture |
| LSBSteg.py | python script file to encode and decode   |

## 2. build environment

use Anaconda to build python environment

```bash
#create environment
conda create -n hw7 python=3.8
#activate environment
conda activate hw7
#intall packages by commad conda
conda install opencv
conda install docopt
```

## 3. modify python script file

```bash
git clone https://github.com/RobinDavid/LSB-Steganography
cd LSB-Steganography/
vi LSBSteg.py
```
modify `LSBSteg.py`
```python
def main():
    args = docopt.docopt(__doc__, version="0.2")
    in_f = args["--in"]
    out_f = args["--out"]
    in_img = cv2.imread(in_f)
    steg = LSBSteg(in_img)
    lossy_formats = ["jpeg", "jpg"]

    if args['encode']:
        #Handling lossy format
        out_f, out_ext = out_f.split(".")
        if out_ext in lossy_formats:
            out_ext = "png"
            print("Output file extension changed to ", out_ext)

        data = open(args["--file"], "rb").read()
        res = steg.encode_binary(data)
        cv2.imwrite(out_f+"."+out_ext, res)

    elif args["decode"]:
        raw = steg.decode_binary()
        with open(out_f, "wb") as f:
            f.write(raw)
```

## 4. hide information

```bash
#modify info.txt with information 
vi info.txt
#hide info.txt into origin.jpg
python LSB-Steganography//LSBSteg.py encode -i origin.jpg -o test.png -f info.txt
#decode hidden information from test.png to info2.txt
python LSB-Steganography//LSBSteg.py decode -i test.png -o info2.txt
```

To verify, only need to compare `info.txt` and `info2.txt`.
