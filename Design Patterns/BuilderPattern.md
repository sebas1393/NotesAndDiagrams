# Builder Pattern
Builder provides an API for constructing an object step-by-step.

¿Why is need it?
  - When objects require a lot of ceremony to create.
  - Having an object with 10 constructor arguments is not productive.

Instead, opt for piecewise construction.

This is an example of a code without a builder a really straightforward code:

    public class WithoutBuilder {
        var hello = "hello";
        var sb = new StringBuilder();
        sb.Append("<p>");
        sb.Append(hello);
        sb.Append("</p>");
        var words = new[] {"hello", "world"};
        sb.Clear();
        sb.Append("<ul>");
        foreach (var word in words)
        {
            sb.AppendFormat("<li>{0}</li>", world);
        }
        sb.Append("</ul>");
        WriteLine(sb);
    }

this is a basic HTMLBuilder

    public HtmlElement
    {
        public string Name, Text;
        public List<HtmlElement> Elements = new List<HtmlElement>();
        private const int indentSize = 2;
        
        public HtmlElement()
        {
        }
        public HtmlElement(string name, string text){
            Name = name ?? throw new ArgumentNullException(paramName: nameof(name));
            Text = text ?? throw new ArgumentNullException(paramName: nameof(text));
        }
        
        private string ToStringImpl(int indent)
        {
            
        }
        public override string ToString()
        {
            var sb = new StringBuilder();
            var i = new string(' ', indentSize * indent);
            sb.AppendLine($"{i}<{Name}>");
            if(!string.IsNullOrWhiteSpace(Text))
            {
                sb.Append(new string(' ', indentSize * (indent + 1)));
            }
            sb.AppendLine($"{i}</{Name}>");
            return sb.ToString();
        }
    }
    public class HtmlBuilder
    {
        
    }