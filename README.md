# Fundamentals of Decision and Data Analytics (SEEM3650) - Practical Exam Report

**Name:** Fung Sai Wa
**Student ID:** 1155194766  
**Due Date:** May 4, midnight

---

## Task 1: Repository Setup

The nanoGPT repository was cloned in Google Colab using the command:
```
!git clone https://github.com/karpathy/nanoGPT
```
The working directory was set to `/content/nanoGPT`, and required dependencies were installed with:
```
!pip install torch numpy transformers datasets tiktoken wandb tqdm
```

---

## Task 2: Train the Shakespeare Character-level Model

The Shakespeare character-level dataset was prepared by running:
```
!python data/shakespeare_char/prepare.py
```
- **Dataset Stats:** 1,115,394 characters, 65 unique characters, 1,003,854 training tokens, 111,540 validation tokens.

The model was trained with default settings using:
```
!python train.py config/train_shakespeare_char.py --compile=False --always_save_checkpoint=True --eval_interval=5000
```
- **Training Details:** 10.65M parameters, 5,000 iterations, final train loss 0.6248, val loss 1.6801.

Samples were generated using:
```
!python sample.py --out_dir=out-shakespeare-char
```

**Generated Shakespeare Character Samples (First 5 Lines):**
```
ANGELO:
And come, my lord,
Straight and you not.

ISABELLA:
It is bar at him that he that did
```

---

## Task 3: Model Architecture Exploration

### Step 3.1: Training with Different Model Sizes

- **Student ID Last 3 Digits (XYZ):** 766  
- **XYZ mod 4:** 766 % 4 = 2  
- **Settings:** Layers = 7, Heads ∈ {2, 3, 5, 7}

Models were trained with `n_layer=7`, `n_embd=210`, `max_iters=1000`, and varying heads:
```
python train.py config/train_shakespeare_char.py --n_layer=7 --n_head={h} --n_embd=210 --compile=False --out_dir=out-shakespeare-l7-h{h} --max_iters=1000 --batch_size=8
```

### Step 3.2: Visualization

- **Plot:** A line graph showing training loss at iteration 1000 for different numbers of heads with fixed layers=7.  
- **Figure:** Saved as `figures/loss_vs_heads.png`.  
- **Losses at Iteration 1000:**
  - Heads=2: Train Loss=2.1299, Val Loss=2.1881
  - Heads=3: Train Loss=2.1136, Val Loss=2.1665
  - Heads=5: Train Loss=2.0735, Val Loss=2.1388
  - Heads=7: Train Loss=2.0775, Val Loss=2.1440

### Step 3.3: Lowest Validation Loss

- **Lowest Validation Loss:** 2.1388  
- **Best Settings:** Layers=7, Heads=5

---

## Task 4: Training BabyGPT for Code Generation

### Step 4.1: Dataset Preparation

- **XYZ mod 2:** 766 % 2 = 0 → C/C++ code  
- **Dataset Source:** Cloned from `https://github.com/The-Young-Programmer/C-CPP-Programming.git`.  
- **Process:** Aggregated `.c`, `.cpp`, `.h` files into `data/code_generation/input.txt`, duplicated to meet token requirement (~132,433 tokens).  
- **Token Count:** 476,445 tokens (after running `prepare.py`), vocab size=111.

### Step 4.2: Training and Generation

The model was trained with a new config `config/train_code_generation.py`:
- Batch size=2, block size=64, n_layer=7, n_head=5, n_embd=210, max_iters=500.  
- Command:
```
python train.py config/train_code_generation.py --compile=False --out_dir=out-code-generation
```
- **Results:** Final train loss=1.0083, val loss=0.9693.

Samples were generated with:
```
python sample.py --out_dir=out-code-generation --start='def '
```

**Generated Code Samples (First 20 Lines):**
```
def boardace
case '\n';
            return ch;
         }

             // colord candition to din tition
           // allt_scording();
               {
                cout << "\t\t3.Restetext\t";
          gotoxy(row, col);
       cout << "\t\t Presssss Ad\n^-----------------------------------------------------------------------------------------------------------------------

			  << "\t\t            "
		     << "\t\t\t\t"
	    < "\t\t\t\t\t                 "
	      << '\n'
	       <<"\n\t\t  "

---------------
def Pase erenot Rewecord the to se Record
/ mainy scalate cublacttion = *
```

**Favorite Generated Snippet(s):**
```
def to ( * 10 & X <= 310 && <= 30 && Y <= 250) // Condition For *// For ahn bele othe for the for
   if (print != "8" || got\n");
   //********************************4
   if (X >= 200 & X <= 280; // Box Cllc Funtion
   {
      system("cls");
      if(char*)& (board[9] - 2))
      {
         if(board[8] == 2) {
            if(boarg[i] == 2)
            gotoxy(30,3);
            printf("|");
            gotoxy(row, col);
            break;
         case '0':
```
This snippet is most interesting one. Since it got the class, and then it has the if else statement which can do the conditional test. 
Moreover, it's look like the boardgame like the x have a range of value. And once the board hit certain location, it would become the boarder "|"
Overall, this is the most structured and fun output with game like logic, despite the errors.

---

**Notes:**

- GitHub repository set to public and submitted via google form already
- Plots saved in `figures/`.  
- Configuration file: `config/train_code_generation.py`.  
- Dataset: `data/code_generation/input.txt`.