import re

def lexical_analyzer(code):
    tokens = {
        'IDENTIFIER': [],
        'CONSTANT': [],
        'COMMENT': [],
        'OPERATOR': []
    }
    
    # Find comments first and remove them from code for other processing
    comment_pattern = r'//.*?$|/\*.*?\*/'
    comments = re.findall(comment_pattern, code, re.MULTILINE | re.DOTALL)
    if comments:
        tokens['COMMENT'] = comments
    
    # Remove comments from code
    code_clean = re.sub(comment_pattern, '', code, flags=re.MULTILINE | re.DOTALL)
    
    # Find constants (numbers, character literals, string literals)
    constants = re.findall(r'\b\d+\b|\b\d+\.\d+\b|\'[^\']\'|\".*?\"', code_clean)
    if constants:
        tokens['CONSTANT'] = constants
    
    # Find operators
    operators = re.findall(r'[+\-*/%=<>!&|^~]=?|\+\+|--|&&|\|\|', code_clean)
    if operators:
        tokens['OPERATOR'] = operators
    
    # Find identifiers (excluding keywords)
    keywords = {'auto', 'break', 'case', 'char', 'const', 'continue', 'default', 'do', 
                'double', 'else', 'enum', 'extern', 'float', 'for', 'goto', 'if', 
                'int', 'long', 'register', 'return', 'short', 'signed', 'sizeof', 
                'static', 'struct', 'switch', 'typedef', 'union', 'unsigned', 'void', 
                'volatile', 'while'}
    
    # Find all words that could be identifiers
    all_words = re.findall(r'\b[a-zA-Z_][a-zA-Z0-9_]*\b', code_clean)
    
    # Filter out keywords
    identifiers = [word for word in all_words if word not in keywords]
    if identifiers:
        tokens['IDENTIFIER'] = identifiers
    
    return tokens

def main():
    print("C Lexical Analyzer (4 Features Only)")
    print("===================================")
    print("Enter C code (press Enter twice to finish):")
    
    lines = []
    while True:
        line = input()
        if not line:
            break
        lines.append(line)
    
    code = '\n'.join(lines)
    
    # Analyze
    result = lexical_analyzer(code)
    
    # Display results
    print("\nResults:")
    print("========")
    for category, items in result.items():
        if items:  # Only print categories that have items
            print(f"{category}: {', '.join(items)}")

if __name__ == "__main__":  # Fixed the condition
    main()



-------------------------------------------------------------------------------------------------
// Sample comment
int main() {
    int x = 10;
    float y = 3;
    x = x + y;  /* Another comment */
}
