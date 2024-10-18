
修改:

cd LLaMA-Factory
pip install -e.[metrics,modelscope,qwen]
set USE_MODELSCOPE_HUB=1
python src/webui.py

To create a public link, set `share=True` in `launch()`. in/src/webui.py

